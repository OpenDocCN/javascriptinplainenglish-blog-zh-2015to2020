# 如何在 Express.js 中处理密码重置

> 原文：<https://javascript.plainenglish.io/how-to-handle-password-reset-in-expressjs-fde6f5015332?source=collection_archive---------8----------------------->

![](img/dc28ce44cf3140c68f2153e884671aa8.png)

没有密码重置功能的认证系统是不完整的。就我个人而言，我绝不会出售不包含这一功能的产品。有必要为用户提供一种在丢失或忘记密码的情况下恢复对其帐户/数据的访问的方法。在本文中，我将演示如何在 ExpressJS 中处理密码重置。

在前两篇文章中，我写了[如何将 ExpressJS 应用程序连接到 MongoDB 数据库](https://kelvinmwinuka.com/how-to-set-up-mongoose-with-expressjs/)和[构建用户注册和认证系统](https://kelvinmwinuka.com/how-to-create-registration-authentication-with-express-passportjs/)。

这两篇文章都与今天的文章有关。我们将使用 mongoose 和我们保存的用户数据来启用密码重置。

如果您已经阅读了这些文章，或者已经有了自己的认证系统，请继续阅读。即使您使用不同的技术，您仍然可以从这种方法中获得一些有价值的想法。

一如既往，这个项目是在 [Github](https://github.com/kelvinmwinuka/express-tutorial) 上托管的。请随意克隆这个项目，以获得我在本文中使用的源代码。

# 密码重置流程

在深入研究代码之前，让我们首先从用户的角度建立密码重置流程，然后设计该流程的实现。

# 用户视角

从用户的角度来看，这个过程应该如下:

1.  点击登录页面中的“忘记密码”链接。
2.  已重定向至需要电子邮件地址的页面。
3.  在电子邮件中接收密码重置链接。
4.  链接重定向到需要新密码和密码确认的页面。
5.  提交后，重定向到带有成功消息的登录页面。

# 重置系统特性

我们还需要了解好的密码重置系统的一些特征:

1.  应该为用户生成唯一的密码重置链接，以便当用户访问该链接时，可以立即识别他们。这意味着在链接中包含一个唯一的令牌。
2.  密码重置链接应有一个过期时间(例如 2 小时)，过期后将不再有效，并且不能用于重置密码。
3.  密码重置后，重置链接应过期，以防止同一链接被用于多次重置密码。
4.  如果用户多次请求更改密码，而没有遵循整个过程，则每个生成的链接都会使前一个无效。这可以防止拥有多个可以重置密码的活动链接。
5.  如果用户选择忽略发送给他们的电子邮件的密码重置链接，他们当前的凭据应该保持不变，并对未来的身份验证有效。

# 实施步骤

我们现在从用户的角度清楚地了解了重置流程以及密码重置系统的特征。以下是我们在实施该系统时将采取的步骤:

1.  创建一个名为“PasswordReset”的 mongoose 模型来管理活动的密码重置请求/令牌。此处设置的记录应在指定的时间段后过期。
2.  在登录表单中包含“忘记密码”链接，该链接会导致包含电子邮件表单的路由。
3.  将电子邮件提交到发布路径后，检查是否存在具有所提供电子邮件地址的用户。
4.  如果用户不存在，重定向回电子邮件输入表单，并通知用户没有找到具有所提供电子邮件的用户。
5.  如果该用户存在，则生成一个密码重置令牌，并将其保存到引用该用户的文档中的 password reset 集合中。如果此集合中已经有与此用户关联的文档，则更新/替换当前文档(每个用户只能有一个文档)。
6.  生成一个包含密码重置令牌的链接，通过电子邮件将该链接发送给用户。
7.  重定向至提示用户检查其电子邮件地址以获取重置链接的成功消息的登录页面。
8.  一旦用户单击该链接，它应该指向一个 GET route，该路由将令牌作为路由参数之一。
9.  在此路由中，提取令牌并查询此令牌的 PasswordReset 集合。如果找不到文档，提醒用户链接无效/过期。
10.  如果找到了文档，加载一个表单来重置密码。该表单应该有 2 个字段(新密码和确认密码字段)。
11.  当提交表单时，它的 post route 会将用户的密码更新为新密码。
12.  删除 password reset 集合中与此用户关联的密码重置文档。
13.  将用户重定向到带有成功消息的登录页面。

# 履行

# 设置

首先，我们必须建立项目。安装用于生成唯一令牌的 [uuid](https://www.npmjs.com/package/uuid) 包和用于发送电子邮件的 [nodemailer](https://www.npmjs.com/package/nodemailer) 包。

```
npm install uuid nodemailer
```

将整个域添加到环境变量中。我们需要它来生成一个填充链接字符串，并通过电子邮件发送给用户。

```
DOMAIN=http://localhost:8000
```

在以下方面对应用程序条目文件进行一些更改:

1.  在 mongoose 连接选项中将“useCreateIndex”设置为“true”。这使得 mongoose 的默认索引构建使用 createIndex 而不是 ensureIndex，并防止 MongoDB 弃用警告。
2.  导入一个包含所有名为“密码重置”的重置路由的新路由文件。我们稍后将创建这些路线。

```
const connection = mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  useCreateIndex: true
})...app.use('/', require('./routes/password-reset'))
```

# 模型

我们需要一个专门的模型来处理密码重置记录。在“模型”文件夹中，使用以下代码创建一个名为“PasswordReset”的模型:

```
const { Schema, model } = require('mongoose')const schema = new Schema({
  user: {
    type: Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  token: {
    type: Schema.Types.String,
    required: true
  }
}, {
  timestamps: true
})schema.index({ 'updatedAt': 1 }, { expireAfterSeconds: 300 })const PasswordReset = model('PasswordReset', schema)module.exports = PasswordReset
```

在这个模型中，我们有两个属性，一个是请求密码重置的用户，另一个是分配给特定请求的惟一令牌。

确保将时间戳选项设置为 true，以便在文档中包括“createdAt”和“updatedAt”字段。

定义模式后，在 updatedAt 字段上创建一个索引，过期时间为 300 秒(5 分钟)。我把它设得这么低是为了测试。在生产中，您可以将这个时间增加到更实际的时间，比如 2 小时。

在我们在本文的[中创建的用户模型(或您当前拥有的用户模型)中，将预保存挂钩更新为以下内容:](https://kelvinmwinuka.com/how-to-set-up-mongoose-with-expressjs/)

```
userSchema.pre('save', async function(next){
  if (this.isNew || this.isModified('password')) this.password = await bcrypt.hash(this.password, saltRounds)
  next()
})
```

这样做是为了确保无论文档是新的还是现有文档中的密码字段已被更改，密码字段都会被散列。

# 路线

在路由的文件夹中创建一个名为“password-reset.js”的新文件。这是我们在应用入口文件中导入的文件。

在此文件中，导入 User 和 PasswordReset 模型。从 uuid 包中导入 v4 函数以生成令牌。

```
const router  = require('express').Router()
const { User, PasswordReset } = require('../models')
const { v4 } = require('uuid')/* Create routes here */module.exports = router
```

创建前两条路线。这些路由与接受用户电子邮件地址的表单相关联。

```
router.get('/reset', (req, res) => res.render('reset.html'))router.post('/reset', async (req, res) => {
  /* Flash email address for pre-population in case we redirect back to reset page. */
  req.flash('email', req.body.email)/* Check if user with provided email exists. */
  const user = await User.findOne({ email: req.body.email })
  if (!user) {
    req.flash('error', 'User not found')
    return res.redirect('/reset')
  }/* Create password reset token and save in collection along with user. 
     If there already is a record with current user, replace it. */
  const token = v4().toString().replace(/-/g, '')
  PasswordReset.updateOne({ 
    user: user._id 
  }, {
    user: user._id,
    token: token
  }, {
    upsert: true
  })
  .then( updateResponse => {
    /* Send email to user containing password reset link. */
    const resetLink = `${process.env.DOMAIN}/reset-confirm/${token}`
    console.log(resetLink)req.flash('success', 'Check your email address for the password reset link!')
    return res.redirect('/login')
  })
  .catch( error => {
    req.flash('error', 'Failed to generate reset link, please try again')
    return res.redirect('/reset')
  })
})
```

第一个是到“/reset”的 GET 路由。在此路径中，呈现“reset.html”模板。我们稍后将创建这个模板。

第二个路由是“/reset”的 POST 路由。此路由要求在请求正文中包含用户的电子邮件。在这条路线中:

1.  闪回电子邮件进行预填充，以防我们重定向回电子邮件表单。
2.  检查提供电子邮件的用户是否存在。否则，刷新错误并重定向回“/reset”。
3.  使用 v4 创建令牌。
4.  更新与当前用户关联的密码重置文档。在选项中将 upsert 设置为 true 以创建一个新文档(如果还没有文档的话)。
5.  如果更新成功，将链接发送给用户，显示成功消息并重定向到登录页面。
6.  如果更新不成功，闪烁一条错误消息并重定向回电子邮件页面。

目前，我们只记录到控制台的链接。我们稍后将实现电子邮件逻辑。

当用户访问上面的链接生成的链接时，创建 2 条路由。

```
router.get('/reset-confirm/:token', async (req, res) => {
  const token = req.params.token
  const passwordReset = await PasswordReset.findOne({ token })
  res.render('reset-confirm.html', { 
    token: token,
    valid: passwordReset ? true : false
  })
})router.post('/reset-confirm/:token', async (req, res) => {
  const token = req.params.token
  const passwordReset = await PasswordReset.findOne({ token })

  /* Update user */
  let user = await User.findOne({ _id: passwordReset.user })
  user.password = req.body.password

  user.save().then( async savedUser =>  {
    /* Delete password reset document in collection */
    await PasswordReset.deleteOne({ _id: passwordReset._id })
    /* Redirect to login page with success message */
    req.flash('success', 'Password reset successful')
    res.redirect('/login')
  }).catch( error => {
    /* Redirect back to reset-confirm page */
    req.flash('error', 'Failed to reset password please try again')
    return res.redirect(`/reset-confirm/${token}`)
  })
})
```

第一个路由是 get 路由，它需要 url 中的令牌。提取令牌，然后进行验证。通过在 PasswordReset 集合中搜索具有所提供令牌的文档来验证令牌。

如果找到了文档，则将“有效”模板变量设置为 true，否则，将其设置为 false。确保将令牌本身传递给模板。我们将在密码重置表单中使用它。

通过按令牌搜索 PasswordReset 集合来检查令牌的有效性。

第二个路由是接受密码重置表单提交的 POST 路由。从 url 中提取令牌，然后检索与之关联的密码重置文档。

更新与此特定密码重置文档关联的用户。设置新密码并保存更新的用户。

更新用户后，删除密码重置文档以防止其被重新用于重置密码。

闪现一条成功消息，并将用户重定向到登录页面，用户可以使用新密码登录。

如果更新不成功，闪现一条错误消息并重定向回相同的表单。

# 模板

一旦我们创建了路线，我们需要创建模板

在“视图”文件夹中，创建一个包含以下内容的“reset.html”模板文件:

```
{% extends 'base.html' %}{% set title = 'Reset' %}{% block styles %}
{% endblock %}{% block content %}
  <form action='/reset' method="POST">
    {% if messages.error %}
      <div class="alert alert-danger" role="alert">{{ messages.error }}</div>
    {% endif %}
    <div class="mb-3">
      <label for="name" class="form-label">Enter your email address</label>
      <input 
        type="text" 
        class="form-control {% if messages.error %}is-invalid{% endif %}" 
        id="email" 
        name="email"
        value="{{ messages.email or '' }}"
        required>
    </div>
    <div>
      <button type="submit" class="btn btn-primary">Send reset link</button>
    </div>
  </form>
{% endblock %}
```

这里我们有一个电子邮件字段，如果在之前的请求中闪现了一个电子邮件值，则该字段会预填充一个电子邮件值。

包括一个警告，如果从以前的请求刷新了一个错误消息，则显示一个错误消息。

在同一文件夹中创建另一个名为“reset-confirm.html”的模板，内容如下:

```
{% extends 'base.html' %}{% set title = 'Confirm Reset' %}{% block content %}
  {% if not valid %}
    <h1>Oops, looks like this link is expired, try to <a href="/reset">generate another reset link</a></h1>
  {% else %}
    <form action='/reset-confirm/{{ token }}' method="POST">
      {% if messages.error %}
        <div class="alert alert-danger" role="alert">{{ messages.error }}</div>
      {% endif %}
      <div class="mb-3">
        <label for="name" class="form-label">Password</label>
        <input 
          type="password" 
          class="form-control {% if messages.password_error %}is-invalid{% endif %}" 
          id="password" 
          name="password">
        <div class="invalid-feedback">{{ messages.password_error }}</div>
      </div>
      <div class="mb-3">
        <label for="name" class="form-label">Confirm password</label>
        <input 
          type="password" 
          class="form-control {% if messages.confirm_error %}is-invalid{% endif %}" 
          id="confirmPassword" 
          name="confirmPassword">
        <div class="invalid-feedback">{{ messages.confirm_error }}</div>
      </div>
      <div>
        <button type="submit" class="btn btn-primary">Confirm reset</button>
      </div>
    </form>
  {% endif %}
{% endblock %}
```

在这个表单中，检查我们在 GET route 中设置的“valid”变量的值，如果为 false，则呈现过期的令牌消息。否则，呈现密码重置表单。

如果在之前的请求中刷新了一个错误消息，则包括显示错误消息的警报。

转到我们在[注册&认证](https://kelvinmwinuka.com/how-to-create-registration-authentication-with-express-passportjs/)文章中创建的登录表单，并将以下代码添加到表单顶部:

```
{% if messages.success %}
    <div class="alert alert-success" role="alert">{{ messages.success }}</div>
{% endif %}
```

这会呈现我们在创建/发送重置链接时以及在重定向到登录页面之前更新用户密码时闪烁的成功消息。

# 邮件

在前面的路由部分中，我们记录了控制台中的重置链接。理想情况下，当用户请求密码重置链接时，我们应该向用户发送电子邮件。

对于这个例子，我使用了 [ethereal.email](https://ethereal.email/) 来生成一个用于开发目的的测试电子邮件帐户。去那里创建一个(这是一个点击过程)。

创建测试帐户后，将以下变量添加到环境变量中:

```
EMAIL_HOST=smtp.ethereal.email EMAIL_NAME=Leanne Zulauf EMAIL_ADDRESS=leanne.zulauf@ethereal.email EMAIL_PASSWORD=aDhwfMry1h3bbbR9Av EMAIL_PORT=587 EMAIL_SECURITY=STARTTLS
```

这些是我写作时的价值观，在这里插入你自己的价值观。

在项目的根目录下创建一个“helpers.js”文件。这个文件将包含一系列有用的函数，这些函数可能会在整个项目中重用。

在这里定义这些函数，这样我们就可以在需要的时候导入它们，而不是在整个应用程序中重复类似的逻辑。

```
const nodemailer = require('nodemailer')module.exports = {
  sendEmail: async ({ to, subject, text }) => {
    /* Create nodemailer transporter using environment variables. */
    const transporter = nodemailer.createTransport({
      host: process.env.EMAIL_HOST,
      port: Number(process.env.EMAIL_PORT),
      auth: {
        user: process.env.EMAIL_ADDRESS,
        pass: process.env.EMAIL_PASSWORD
      }
    })
    /* Send the email */
    let info = await transporter.sendMail({
      from: `"${process.env.EMAIL_NAME}" <${process.env.EMAIL_ADDRESS}>`,
      to,
      subject,
      text
    })
    /* Preview only available when sending through an Ethereal account */
    console.log(`Message preview URL: ${nodemailer.getTestMessageUrl(info)}`)
  }
}
```

导出具有各种功能的对象。第一个是“发送电子邮件”功能。

该函数接收收件人的地址、电子邮件主题和电子邮件文本。使用之前在选项中定义的环境变量创建节点邮件传输器。使用传递给该函数的参数发送电子邮件。

该函数的最后一行将消息 url 记录在控制台中，以便您可以在 Ethereal mail 上查看消息。测试帐户实际上并不发送电子邮件。

返回“password-reset.js”路线，添加电子邮件功能。首先，导入函数:

```
const { sendEmail } = require('../helpers')
```

在“/reset”POST 路由中，添加以下代码，而不是在控制台上记录 reset 链接:

```
sendEmail({
      to: user.email, 
      subject: 'Password Reset',
      text: `Hi ${user.name}, here's your password reset link: ${resetLink}. 
      If you did not request this link, ignore it.`
    })
```

成功更新用户后，再发送一封电子邮件，通知用户在“/重置-确认”发布路径中成功更改了密码:

```
user.save().then( async savedUser =>  {
    /* Delete password reset document in collection */
    await PasswordReset.deleteOne({ _id: passwordReset._id })
    /* Send successful password reset email */
    sendEmail({
      to: user.email, 
      subject: 'Password Reset Successful',
      text: `Congratulations ${user.name}! Your password reset was successful.`
    })
    /* Redirect to login page with success message */
    req.flash('success', 'Password reset successful')
    res.redirect('/login')
  }).catch( error => {
    /* Redirect back to reset-confirm page */
    req.flash('error', 'Failed to reset password please try again')
    return res.redirect(`/reset-confirm/${token}`)
  })
```

# 结论

在本文中，我演示了如何使用 NodeMailer 在 ExpressJS 中实现密码重置特性。

在下一篇文章中，我将讲述如何在您的 Express 应用程序中实现一个用户电子邮件验证系统。我将使用与本文类似的方法，选择 NodeMailer 作为电子邮件包。

**如果你喜欢这篇文章，可以考虑关注我的** [**个人网站**](https://kelvinmwinuka.com/) **，以便在我的内容在媒体上发布之前提前获取(别担心，它仍然是免费的，没有烦人的弹出广告！).另外，请随意评论这篇文章。我很想听听你们的想法！**

*原载于 2020 年 12 月 24 日 https://kelvinmwinuka.com**的* [*。*](https://kelvinmwinuka.com/how-to-handle-password-reset-in-expressjs/)