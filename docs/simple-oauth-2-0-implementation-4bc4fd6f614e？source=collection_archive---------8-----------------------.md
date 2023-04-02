# 简单的 OAuth 2.0 实现

> 原文：<https://javascript.plainenglish.io/simple-oauth-2-0-implementation-4bc4fd6f614e?source=collection_archive---------8----------------------->

OAuth 是一个开放的授权协议，允许 Twitter、GitHub 或其他 Web 平台上的消费者应用程序访问资源所有者的资源。IETF OAuth 工作组在 2012 年开发了 OAuth 2.0。

![](img/bdc94cf256b9de59d380135c90eb7ac5.png)

Overview of OAuth 2.0 Process

OAuth 2.0 关注产品开发人员的可用性，为 web 应用程序、桌面应用程序、移动电话和客厅中的设备提供独特的授权流程。

OAuth 2.0 框架为不同的用例定义了几种授权类型，以及创建新授权类型的框架。在 OAuth 2.0 中，术语“授权类型”指的是应用程序获取访问令牌的方式。

OAuth 2.0 的基本授权类型如下:

*   授权代码
*   密码
*   客户端凭据
*   刷新令牌

# **表格结构**

```
**users**| Column                  | Data Type
|----------               |-------------
| id                      | int
| name                    | string
| username                | string
| password                | string**client**| Column                  | Data Type
|----------               |-------------
| id                      | int
| client_id               | string
| client_secret           | string
| name                    | string
| home_page_url           | string
| logo_url                | string
| privacy_policy_url      | string
| user_id                 | string
| is_live                 | bool
| redirect_uri            | []string**client_access_tokens**| Column                  | Data Type
|----------               |-------------
| id                      | int
| client_id               | int
| user_id                 | int
| client_refresh_token_id | int
| access_token            | string
| grant_type              | string
| scope                   | string
| audience                | string
| expired_at              | datetime **client_refresh_tokens**| Column                  | Data Type
|----------               |-------------
| id                      | int
| client_id               | int
| user_id                 | int
| refresh_token           | string
| grant_type              | string
| scope                   | string
| audience                | string
| expired_at              | datetime **client_authorization_codes**| Column                  | Data Type
|----------               |-------------
| id                      | int
| client_id               | int
| user_id                 | int
| code                    | string
| scope                   | []string
| is_used                 | bool
| redirect_uri            | string
| expired_at              | datetime
```

# 客户端凭据授予

对于这个请求，客户端需要验证自己。通常，该服务将允许接受 HTTP 基本 auth 报头中的客户端 ID 和秘密。

**请求**

```
POST /oauth/token
Host: authorization-server.com

// header
Authorization: Basic base64_encode(client_id:client_secret) // required

// body
{
  grant_type: "client_credentials", // required
  scope: "get_user_profile" // optional, but if not passed will give full access of the client user
}
```

**回应**

```
// body
{
   tokenType: "Bearer",
   expiresIn: 3600,
   accessToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
   refreshToken: "eqweasdasdanvbSifkLImUAtuXITjxKMzkXjZvKGGIcLyFqHIDrVNexjHwXBNRhgicutwNStdasdasdLmMjfggjHrYvzWaIANmUvNOoDtNIOKOFywqedsa",
   scope: "get_user_profile"
}
```

# 密码授权

如果客户端获得了一个密码，那么客户端必须验证这个请求。通常，该服务将允许接受 HTTP 基本 auth 报头中的客户端 ID 和秘密。

**请求**

```
POST /oauth/token
Host: authorization-server.com

// header
Authorization: Basic base64_encode(client_id:client_secret) // optional

// body
{
  grant_type: "password", // required
  username: "yussuf", // required
  password: "123456" // required
  scope: "get_user_profile" // optional, but if not passed will give full access of the user
}
```

**回应**

```
// body
{
   tokenType: "Bearer",
   expiresIn: 3600,
   accessToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
   refreshToken: "eqweasdasdanvbSifkLImUAtuXITjxKMzkXjZvKGGIcLyFqHIDrVNexjHwXBNRhgicutwNStdasdasdLmMjfggjHrYvzWaIANmUvNOoDtNIOKOFywqedsa",
   scope: "get_user_profile"
}
```

# 授权码授予

授权码是一个临时码，客户端将使用它来交换访问令牌。代码本身是从授权服务器获得的，用户有机会看到客户端请求的信息，并批准或拒绝请求。

web 流的第一步是向用户请求授权。这是通过为用户创建一个授权请求链接来实现的。

授权 URL 通常采用如下格式:

> https://authorization-server.com/oauth/authorize?client _ id = abcdefgh & response _ type = code & state = testing & redirect _ uri = https % 3A % 2F % 2f testing . com % 0A & scope = get _ user _ profile

在用户批准后，它应该以这种格式重定向，生成的代码只在一次与服务器交换以获得访问令牌时有效:

> https://testing.com？代码= abc123 &状态=测试

**请求**

```
POST /oauth/token
Host: authorization-server.com

// header
Authorization: Basic base64_encode(client_id:client_secret) // required

// body
{
  grant_type: "authorization_code", // required
  code: "abc123", // required get from the redirect url code query string
  redirect_uri: "https://testing.com" // required
}
```

**响应**

```
// body
{
   tokenType: "Bearer",
   expiresIn: 3600,
   accessToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
   refreshToken: "eqweasdasdanvbSifkLImUAtuXITjxKMzkXjZvKGGIcLyFqHIDrVNexjHwXBNRhgicutwNStdasdasdLmMjfggjHrYvzWaIANmUvNOoDtNIOKOFywqedsa",
   scope: "get_user_profile"
}
```

# 刷新令牌授权

如果客户端获得了一个密码，那么客户端必须验证这个请求。通常，该服务将允许接受 HTTP 基本 auth 报头中的客户端 ID 和秘密。

该刷新令牌端点用于通过获取新的访问令牌来更新访问令牌。此端点对于授权代码授权非常重要，授权代码授权需要用户交互才能获得新的访问令牌，此授权将帮助您通过使用用户首次批准应用程序请求时获得的刷新令牌来跳过用户交互。

**请求**

```
POST /oauth/token
Host: authorization-server.com

// header
Authorization: Basic base64_encode(client_id:client_secret) // optional

// body
{
  grant_type: "refresh_token", // required
  refresh_token: "eqweasdasdanvbSifkLImUAtuXITjxKMzkXjZvKGGIcLyFqHIDrVNexjHwXBNRhgicutwNStdasdasdLmMjfggjHrYvzWaIANmUvNOoDtNIOKOFywqedsa", // required
  scope: "get_user_profile" // optional, the requested scope must not include additional scopes that were not issued in the original access token, can omit some scope from the original access token.
}
```

**响应**

```
// body
{
   tokenType: "Bearer",
   expiresIn: 3600,
   accessToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5cnewtoken",
   refreshToken: "eqweasdasdanvbSifkLImUAtuXITjxKMzkXjZvKGGIcLyFqHIDrVNexjHwXBNRhgicutwNStdasdasdLmMjfggjHrYvzWaIANmUvNOoDtNIOKOFywqedsanewrefreshtoken",
   scope: "get_user_profile"
}
```

# 结论

每个公司对于自己的 OAuth 标准都有不同的实现，但是本文基于 IETF OAuth 工作组发布的 OAuth 2.0 授权框架( **RFC6749** )。

# **参考**

【https://www.oauth.com/ 

[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749)