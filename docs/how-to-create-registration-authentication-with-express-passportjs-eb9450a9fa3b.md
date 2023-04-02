# 如何使用 Express & PassportJS 创建注册和认证

> 原文：<https://javascript.plainenglish.io/how-to-create-registration-authentication-with-express-passportjs-eb9450a9fa3b?source=collection_archive---------22----------------------->

![](img/f31943303d0b1408eb74b980d8b52c4a.png)

在本文中，我将演示如何在 ExpressJS 中构建一个用户注册和认证系统。在上一篇文章中，我们[使用 mongose](https://kelvinmwinuka.com/how-to-set-up-mongoose-with-expressjs/)建立了一个 MongoDB 连接。在这里，我们将使用该连接来保存用户数据并使用它进行身份验证。

这个项目在 [Github](https://github.com/kelvinmwinuka/express-tutorial) 上有。如果你愿意的话，可以随意复制它。

# 设置

让我们首先为项目的这一部分设置必要的包和库。

运行以下命令安装必要的软件包:

```
npm install passport passport-local express-session bcrypt connect-mongo express-flash joi
```

这是我们刚刚安装的软件包的明细:

1.  [护照](http://www.passportjs.org/)和[护照-本地](http://www.passportjs.org/packages/passport-local/)-用户认证。
2.  [快速会话](https://www.npmjs.com/package/express-session)—express js 中的会话。
3.  [bcrypt](https://www.npmjs.com/package/bcrypt) —密码加密和认证比较。
4.  [connect-mongo](https://www.npmjs.com/package/connect-mongo) —用于快速会话的 mongo 商店。
5.  [快速闪烁](https://www.npmjs.com/package/express-flash) —在前端显示闪烁信息。
6.  [joi](https://joi.dev/api/?v=17.3.0) —用户输入验证。

包含 bootstrap(可选，只要表单可以向服务器发送 post 数据，就可以)。

在***base.html***文件中，添加引导导入的链接和脚本标签。它们被导入一次，然后包含在扩展基本模板的每个模板中。

在这个阶段，base.html 的文件应该是这样的:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>{{ title }}</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- Bootstrap CSS -->
    <link 
      href="[https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css](https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css)" 
      rel="stylesheet" 
      integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" 
      crossorigin="anonymous">
    {% block styles %}
      {# This block will be replaced by child templates when importing styles #}
    {% endblock %}
  </head>
  <body>
    {% block content %}
      {# This block will be replaced by child templates when adding content to the  #}
    {% endblock %}<!-- Bootstrap JavaScript Bundle with Popper -->
    <script 
      src="[https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js](https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js)" 
      integrity="sha384-ygbV9kiqUc6oa4msXn9868pTtWMgiQaeYH7/t7LECLbyPA2x65Kgf80OJFdroafW" 
      crossorigin="anonymous">
    </script>
    {% block scripts %}
      {# This block will be replaced by child templates when importing scripts #}
    {% endblock %}
  </body>
</html>
```

# 履行

进入入口点文件并需要以下包:

```
const session = require('express-session') 
const MongoStore = require('connect-mongo')(session) 
const passport = require('passport')
```

在应用程序声明之后，添加内置的 express 中间件来解析带有 url 编码数据的传入请求，以处理将从表单接收的数据。

```
var app = express() 
app.use(express.urlencoded({extended: true}))
```

接下来，设置会话中间件。确保将这段代码放在 mongose 连接之后，因为我们将使用现有的 mongose 连接来存储会话数据。否则，您必须为此创建一个新连接。

```
app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: true,
  store: new MongoStore({
    mongooseConnection: mongoose.connection,
    collection: 'sessions'
  }),
  cookie: {
    secure: false
  }
}))
```

让我们浏览一下上面的代码:

1.  我们正在将会话中间件添加到应用中。
2.  secret —用于加密会话的字符串。在。env 文件或系统环境变量。
3.  重新保存—确定是否将会话对象重新保存到会话存储中，即使该对象未被请求修改。
4.  save initialized—确定是否应该将新会话保存到存储中，甚至在修改之前。
5.  存储—用于保存会话数据的存储。

# 更新模型

在这一节中，我指的是我们在上一篇文章中创建的用户模型。[看这里](https://kelvinmwinuka.com/how-to-set-up-mongoose-with-expressjs/)。

现在我们需要更新用户模型，以便在保存时启用身份验证和密码散列。我们在模型中这样做是为了避免在需要时在多个地方编写认证登录。

这个逻辑是这个模型所独有的，所以在这里使用它是有意义的。导航到我们之前创建的 User.js 模型文件，并在第一个 require 语句后添加以下代码:

```
const bcrypt = require('bcrypt') 
const saltRounds = 10
```

在模式定义之后，添加以下代码:

```
userSchema.pre('save', async function(next){
  if (this.isNew) this.password = await bcrypt.hash(this.password, saltRounds)
  next()
})userSchema.static('userExists', async function({username, email}){
  let user = await this.findOne({ username })
  if (user) return { username: 'This username is already in use' }
  user = await this.findOne({ email })
  if (user) return { email: 'This email address is already in use' }
  return false
})userSchema.static('authenticate', async function(username, plainTextPassword){
  const user = await this.findOne({ $or: [ {email: username}, {username} ] })
  if (user && await bcrypt.compare(plainTextPassword, user.password)) return user
  return false
})
```

这里发生了一些事情:

1.  第一个是预保存挂钩。这将在每次保存文档之前运行。我们用它来确定当前文档是否是新的(不是更新调用)。如果文档是新的，散列密码。始终保存哈希密码，而不是纯文本。
2.  第二个块是一个静态方法，它检查用户是否存在。我们将通过用户名查询数据库，然后发送电子邮件。如果找到用户，则返回一个对象，指定哪个用户已经在使用。否则，返回 false。
3.  第三种方法是添加到模式中的静态方法。我们用这个来验证用户。如果用户存在，并且明文密码和散列用户密码之间的密码比较通过，则返回用户对象。否则，为返回 false。认证失败。

# 登记

创建注册表；一个简单的表格，收集用户的姓名，用户名，电子邮件地址和密码。

将此代码放在 views 文件夹的“register.html”中。

```
{% extends 'base.html' %}{% set title = 'Register' %}{% block styles %}
  <style>
    form {
      margin-top: 20px;
      margin-left: 20px;
      margin-right: 20px;
    }
  </style>
{% endblock %}{% block content %}
  <form action="/register" method="POST">
    <div class="mb-3">
      <label for="name" class="form-label">Name</label>
      <input 
        type="text" 
        class="form-control {% if messages.name_error %}is-invalid{% endif %}" 
        id="name" 
        name="name"
        value="{{ messages.name or '' }}"
        placeholder="Full Name">
      <div class="invalid-feedback">{{ messages.name_error }}</div>
    </div>
    <div class="mb-3">
      <label for="username" class="form-label">Username</label>
      <input 
        type="text" 
        class="form-control {% if messages.username_error %}is-invalid{% endif %}" 
        id="username" 
        name="username"
        value="{{ messages.username or '' }}"
        placeholder="Username">
      <div class="invalid-feedback">{{ messages.username_error }}</div>
    </div>
    <div class="mb-3">
      <label for="email" class="form-label">Email address</label>
      <input 
        type="email" 
        class="form-control {% if messages.email_error %}is-invalid{% endif %}" 
        id="email"
        name="email"
        value="{{ messages.email or '' }}"
        placeholder="Email Address">
      <div class="invalid-feedback">{{ messages.email_error }}</div>
    </div>
    <div class="mb-3">
      <label for="password" class="form-label">Password</label>
      <input 
        type="password" 
        class="form-control {% if messages.password_error %}is-invalid{% endif %}" 
        id="password" 
        name="password" 
        value="{{ messages.password or '' }}"
        placeholder="Password">
      <div class="invalid-feedback">{{ messages.password_error }}</div>
    </div>
    <div>
      <button type="submit" class="btn btn-primary">Sign me up!</button>
    </div>
  </form>
{% endblock %}{% block scripts %}
{% endblock %}
```

我们使用 nunjucks 来实现一些动态行为。

第一个是使用来自服务器的 flash 消息将 is-invalid 类添加到表单控件中。这将添加一条附加到表单控件的错误信息。

第二个是设置用户输入的前一个值(对于本教程来说，这是一个可选的 UX 特性)。

创建注册模板后，创建与模板相关联的路由。

在项目的根目录下创建一个名为“routes”的文件夹。这个文件夹将保存我们所有的路线。在该文件夹中创建一个文件“register.js”。该文件的内容应该如下所示:

```
var router = require('express').Router()
const Joi = require('joi')
const { User } = require('../models')const validateRegistrationInfo = async (req, res, next) => {
  for(let [key, value] of Object.entries(req.body)) {
    req.flash(`${key}`, value)
  }
  /* Validate the request parameters.
  If they are valid, continue with the request.
  Otherwise, flash the error and redirect to registration form. */
  const schema = Joi.object({
    name: Joi.string().required(),
    username: Joi.string().alphanum().min(6).max(12).required(),
    email: Joi.string()
        .email({ minDomainSegments: 2, tlds: { allow: ['com', 'net'] } }).required(),
    password: Joi.string().min(8).required()
  })const error = schema.validate(req.body, { abortEarly: false }).error
  if (error) {
    error.details.forEach(currentError => {
      req.flash(`${currentError.context.label}_error`, currentError.message)
    })
    return res.redirect('/register')
  }/** Check if user exists */
  const userExists = await User.userExists(req.body)
  if (userExists) {
    for(let [key, message] of Object.entries(userExists)) {
      req.flash(`${key}`, message)
    }
    return res.redirect('/register')
  }next()  
}router.get('/register', (req, res) => res.render('register.html'))router.post('/register', validateRegistrationInfo, async (req, res) => {
  let savedUser = await (new User(req.body)).save()
  res.redirect('/')
})module.exports = router
```

第一个重要的代码块是一个名为***validateRegistrationInfo***的函数。这是用于验证用户注册信息的中间件。

在验证的第一阶段，我们会立即刷新预填充的当前信息，以防我们重定向回注册页面。

阶段 2 是根据验证模式验证每个条目。 [Joi](https://joi.dev/) 包让这个过程变得简单。

如果验证有任何错误，在重定向到注册页面之前，刷新该特定条目的每个错误消息。在模板中显示此错误消息。

验证的最后阶段是检查所提供的用户名/电子邮件是否已经被使用。如果是，则在重定向到注册路由之前闪烁错误消息。

创建一个简单呈现“register.html”的 GET 路由。这是验证失败时我们重定向到的路由。

创建一个 post 路由，接收用户在请求正文中输入的数据，并将验证中间件传递给它。

在路由处理程序本身中，我们不必担心无效数据，因为如果处理程序正在执行，它会通过所有的验证检查。

使用提供的数据创建一个新用户，保存它，并重定向到主页。

导出此路由器对象，并将其导入条目文件，如下所示:

```
// Import rotues 
app.use('/', require('./routes/register'))
```

# 证明

既然我们已经完成了注册，那么是时候实现应用程序的认证逻辑了。

首先创建一个登录表单。该表单有一个用户名/电子邮件字段和一个密码字段。我们还将包含一个条件，用于检查警报中显示的错误消息。闪烁一条消息后，当我们重定向到登录页面时，就会显示这条消息。

将此表单放在注册模板旁边的视图文件夹中的“login.html”模板文件中。

```
{% extends 'base.html' %}{% set title = 'Login' %}{% block styles %}
  <style>
    form {
      margin-top: 20px;
      margin-left: 20px;
      margin-right: 20px;
    }
  </style>
{% endblock %}{% block content %}
  <form action="/login" method="POST">
    {% if messages.error %}
      <div class="alert alert-danger" role="alert">{{ messages.error }}</div>
    {% endif %}
    <div class="mb-3">
      <label for="name" class="form-label">Username or Email</label>
      <input 
        type="text" 
        class="form-control {% if messages.name_error %}is-invalid{% endif %}" 
        id="username" 
        name="username"
        value="{{ messages.name or '' }}">
      <div class="invalid-feedback">{{ messages.name_error }}</div>
    </div>
    <div class="mb-3">
      <label for="name" class="form-label">Password</label>
      <input 
        type="password" 
        class="form-control {% if messages.name_error %}is-invalid{% endif %}" 
        id="password" 
        name="password"
        value="{{ messages.name or '' }}">
      <div class="invalid-feedback">{{ messages.name_error }}</div>
    </div>
    <div>
      <button type="submit" class="btn btn-primary">Login</button>
    </div>
  </form>
{% endblock %}{% block scripts %}
{% endblock %}
```

下一个任务是定义用于认证用户的 passport 策略。我们使用 passport-local 的策略，因为我们根据自己存储的用户凭据进行身份验证。

在项目的根目录下创建一个名为“passport-helper.js”的新文件，内容如下:

```
const LocalStrategy = require('passport-local').Strategy
const { User } = require('./models')module.exports = (app, passport) => {passport.use(new LocalStrategy((username, password, done) => {
    User.authenticate(username, password)
    .then( user => {
      done(null, user)
    })
    .catch( error => {
      done(error)
    })
  }))passport.serializeUser((user, done) => {
    done(null, user._id)
  })passport.deserializeUser((id, done) => {
    User.findById(id, (error, user) => {
      if (error) return done(error)
      done(null, user)
    })
  })app.use(passport.initialize())
  app.use(passport.session())
}
```

第一步是导入策略和用户模型。

第二步是配置策略。我们创建一个新的策略实例，向它传递一个接受用户名、密码的函数和一个验证回调(done)函数，该函数在身份验证过程完成后执行。

身份验证逻辑放在这个函数中。为了保持简洁，我们将简单地使用我们在用户模型中创建的“authenticate”静态方法。

在 passport 中进行身份验证时，如果身份验证成功，用户对象将被传递给 verify 回调函数，否则将返回 false(如果在这种情况下没有抛出错误，则传递错误)。

如果找到用户，我们的 authenticate 方法返回一个用户对象，否则返回 false，因此它的输出非常适合这个场景。

一旦我们配置了策略，我们必须指定用户序列化和反序列化逻辑。

如果您不使用会话，这一步是可选的，但是我们试图创建一个带有会话的登录系统，所以在我们的例子中，这是必要的。

serializeUser 方法将带有用户对象和回调的函数作为参数，该函数确定将存储在会话本身中的数据。

为了保持会话中存储的数据量较小，我们只在会话中存储用户 ID。这个序列化过程发生在初次登录时。

deserializeUser 方法采用一个接收用户 ID 和回调的函数。该方法在登录/序列化后的所有后续请求上运行。

从会话中获取用户 ID，从数据库中检索用户。一旦检索到用户，它们就存储在 req.user 中。

在序列化/反序列化之后，确保将 passport initialize 和会话中间件添加到应用程序中。我们将把所有这些打包到一个函数中，该函数将我们的 app 和 passport 对象作为参数。

我们的 passport 配置现在已经完成。下一步是初始化 passport。

在应用程序入口文件中，导入我们在上一步中创建的函数，然后执行它，传递 app 和 passport 对象。

确保在 passport require 语句之后有 require 语句。在定义会话中间件之后，必须调用初始化函数，因为 passport 会话中间件使用它。

```
const initializePassport = require('./passport-helper') ... initializePassport(app, passport)
```

现在让我们创建登录路由。在 routes 文件夹中，创建一个名为“login.js”的文件，并添加以下代码:

```
const createLoginRoutes = passport => {
  const router = require('express').Router()router.get('/login', (req, res) => {
    if (req.isAuthenticated()) return res.redirect('/')
    res.render('login.html')
  })

  router.post(
    '/login',
    passport.authenticate('local', {
      failureRedirect: '/login', 
      successRedirect: '/',
      failureFlash: 'User not found', 
    }),
    (error, req, res, next) => {
      if (error) next(error)
    }
  )router.get('/logout', (req, res) => {
    req.logout()
    res.redirect('/login')
  })

  return router
}module.exports = createLoginRoutes
```

与我们在注册路由文件中创建路由的方式不同，我们在这里做的有点不同。

因为我们需要 passport 对象，所以我们将导出一个接受 passport 对象作为参数的函数，定义路由并返回 router 对象。

第一个路由是“/login”的 GET 路由。这将在没有活动会话时呈现表单。使用 passport 在请求对象中提供的“isAuthenticated”方法来确定当前是否有活动会话。

第二个路由是来自“/login”的 POST 路由。这个路径接受来自用户的表单输入。

将 passport.authenticate 中间件传递给此路由以处理身份验证。这个中间件接受策略类型和选项对象。

在选项对象中，指定失败和成功时的重定向路径。failureFlash 属性指定身份验证失败时要闪烁的消息。这是您应该检查并显示在登录页面上的消息。

最后，创建一个注销路由，调用 req.logout 来结束当前用户的会话。passport 也提供了这种注销方法。

现在，在条目文件中导入登录路由创建者，并将 passport 对象传递给它:

```
app.use('/', require('./routes/auth')(passport))
```

将主页路径更新为:

```
app.get('/', async (req, res) => {
  if (!req.isAuthenticated()) return res.redirect('/login')
  res.render('home.html')
})
```

主页路由现在是受保护的路由。这意味着只有经过身份验证的用户才能访问它。

我们通过使用 req.isAuthenticated 方法来确保用户已经过身份验证，从而实现了这一点。如果没有，重定向到登录页面。

回到注册路由文件并更新获取路由。致以下内容:

```
router.get('/register', (req, res) => {
  if (req.isAuthenticated()) return res.redirect('/')
  res.render('register.html')
})
```

如果用户没有通过身份验证，我们只需要呈现注册表单，否则，重定向到主页。

# 结论

在本文中，我演示了如何使用 PassportJS 在 ExpressJS 中创建一个简单的注册/认证系统。然而，如果没有密码重置功能，认证系统是不完整的。

下一篇文章将是关于使用 mongoose 和 NodeMailer 创建密码重置特性的教程。

**如果你喜欢这篇文章，可以考虑关注我的** [**个人网站**](https://kelvinmwinuka.com/) **，以便在我的内容在媒体上发布之前提前获得(别担心，它仍然是免费的，没有烦人的弹出广告！).另外，请随意评论这篇文章。我很想听听你们的想法！**

*原载于 2020 年 12 月 21 日 https://kelvinmwinuka.com*[](https://kelvinmwinuka.com/how-to-create-registration-authentication-with-express-passportjs/)**。**