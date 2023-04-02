# 火绒风格的电影夜采摘与 VueJs，Vuetify，VueFire，和 Firebase

> 原文：<https://javascript.plainenglish.io/tinder-style-movie-night-picker-with-vuejs-vuetify-vuefire-and-firebase-part-3-partners-dbeebfa97572?source=collection_archive---------17----------------------->

## 第 3 部分—合作伙伴

![](img/6913a2638ee75ebf44b4b15bcdd33395.png)

Image by [Devon Breen](https://pixabay.com/users/dbreen-1643989/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1085072) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1085072)

在[第二部分](https://medium.com/javascript-in-plain-english/tinder-style-movie-night-picker-with-vuejs-vuetify-vuefire-and-firebase-part-2-authentication-6969c27cdad0)中，我们在电影《夜盗》中设置了认证。现在我们可以开始关注核心逻辑了。我们要做的第一件事是添加一个合作伙伴。完成之后，我们将开始电影的展示和投票。

# 添加合作伙伴

为了在我们的系统中进行匹配，我们需要链接合作伙伴。现在，我们将允许用户选择一个合作伙伴。如果你想把它用于朋友，可以随意扩展到多个伙伴。

要添加合作伙伴，我们将在系统中搜索电子邮件。当我们找到一个匹配，我们将允许用户添加他们为合作伙伴。为了简单起见，我们不会从被添加的用户创建一个验证系统。但是，这将是一个非常好的功能，可以阻止用户未经允许添加其他人。

## Vuex 商店

由于我们将所有用户信息存储在 Vuex 商店中，我们需要做的第一件事是将我们合作伙伴的 id 添加到其中。在 src/store/modules/user.js 中，让我们添加一个名为 partnerId 的新状态属性。

```
const *state* ={
 *id*:'',
 *name*:'',
 *email*:'',
 ***partnerId*:''**
}
```

接下来，我们将更新我们的 SET_USER_DATA 变异，并添加一个名为 SET_PARTNER_ID 的新变异。

```
const *mutations* ={
 *SET_USER_DATA*:(*state*, *payload*)=>{
  *state.id* = *payload.id
  state.name* = *payload.name
  state.email* = *payload.email* ***state.partnerId* = *payload.partnerId***},
 ***SET_PARTNER_ID*:(*state*, *payload*)=>{
  *state.partnerId* =payload
 }**
}
```

最后，我们将添加一个名为 setPartnerId 的新操作，我们可以在添加或更新我们的合作伙伴时使用它:

```
const *actions* ={
 *setUserData*(*context*, *userData*){
  *context.commit*('SET_USER_DATA',userData)
 },
 ***setPartnerId*(*context*, *id*){
  *context.commit*('SET_PARTNER_ID',id)
 }**
}
```

## 添加合作伙伴页面

为了添加这个特性，我们将在 src/views 下创建一个名为 AddPartner.vue 的新文件。我们的用户将通过电子邮件搜索其他用户。如果他们找到一个匹配并添加他们，我们将在我们的用户表中一个名为 partner 的字段下创建一个条目。如果您想要多个伙伴，我建议您要么使用一个名为 partners 的数组，要么使用一个子集合。

## 登录页面更新

我们需要做的最后一件事是在用户登录时将我们的 partnerId 传递给商店。回到 src/views/SignIn.vue，将 partnerId 添加到我们的商店行动分派中:

```
this*.$store.dispatch*('user/setUserData', {
 id: *dbUser.*id,
 name: *userData.*name,
 email: *userData.*email,
 **partnerId: *userData.*partnerId || ''**
})
```

## 路由器

现在页面已经设置好了，让我们将它添加到路由器中。在“路线”下，添加以下内容:

```
{
 *path*:'/add-partner',
 *name*:'Add Partner',
 *beforeEnter*:guard,
 *component*:()=>
  *import*(/* *webpackChunkName: "add-partner"* */ '../views/AddPartner.vue')
}
```

## 航行

要访问此页面，我们需要将其添加到我们的导航中。因此，请转到 src/App.vue，将它添加到应用程序栏中，就在 h2:

```
<v-spacer></v-spacer>
<v-btn *text* *small* *color*="white" *to*="/add-partner">Add Partner</v-btn>
```

# 视频教程

Video Tutorial

# 结论

我们的应用进展非常顺利。到目前为止，我们已经进行了身份验证和合作伙伴选择。在[的下一篇文章](https://diligentdev.medium.com/tinder-style-movie-night-picker-with-vuejs-and-firebase-60a1c5200c)中，我们将致力于让电影进入我们的应用程序，并对它们进行投票。请务必关注我，以便在发布时得到通知。