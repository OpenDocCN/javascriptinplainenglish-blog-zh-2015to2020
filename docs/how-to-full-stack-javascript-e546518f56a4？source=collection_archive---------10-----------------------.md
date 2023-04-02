# 如何使用全堆栈 JavaScript

> 原文：<https://javascript.plainenglish.io/how-to-full-stack-javascript-e546518f56a4?source=collection_archive---------10----------------------->

![](img/219e15f8990f91ac01f367af6833d57d.png)

JavaScript 使桌面和移动设备上的浏览器能够与网站交互。包和框架的 eco 系统使开发人员能够编写代码来更好地帮助开发人员。近年来，在浏览器以外的地方使用 JavaScript 变得很流行。

> JavaScript 已经成为互联网的语言。任何有想法的人都可以通过使用 JavaScript 快速创建、构建和部署全堆栈应用程序。

完整堆栈意味着我们在浏览器上创建、构建和部署服务，比如 Chrome 或 Safari，并托管连接到数据库和处理事件的后端服务器。

## 前端

*   应用程序的外观和感觉。
*   它将如何与用户互动
*   提供服务。

## 后端

*   服务器 Api
*   数据存储。
*   它将如何认证用户和保护人们的数据。
*   将数据发送到前端进行显示。

现在我们已经定义了我们的术语，让我们构建一个可以让我们很快构建的初始项目。

在本教程中，我们将设置一个 mono repo，使用 Docker 来配置开发环境，这样我们就可以开始创建、构建和部署我们的应用程序。

[](https://github.com/andrewgbliss/beta-access) [## andrewgbliss/beta-access

### 通过在 GitHub 上创建一个帐户，为 andrewgbliss/beta-access 开发做出贡献。

github.com](https://github.com/andrewgbliss/beta-access) 

这是这个项目的报告。

让我们谈谈回购的结构，我们将使用哪些服务，以及需要创建哪些文件夹和文件。

*   Api —此文件夹包含将运行 ExpressJs 服务器的 NodeJs 项目。连接到一个数据库，并创建一个供我们在前端使用的 Api。
*   客户端-该文件夹包含将用于前端的 ReactJs 项目。
*   数据库—该文件夹包含用于 Docker 连接到本地数据库的开发脚本。
*   Nginx —此文件夹包含负载平衡器 Nginx，因此我们可以在本地主机上为所有服务进行开发。

我们可以在技术上设置任何我们想要的服务。为它创建一个文件夹，并将其添加到 docker-composite . yml 中

[](https://docs.docker.com/compose/) [## 码头工人写作概述

### 寻找撰写文件参考？在这里找到最新版本。撰写是一个定义和运行…

docs.docker.com](https://docs.docker.com/compose/) 

现在让我们在 docker-composite . yml 中讨论我们的服务。

```
version: "3"
services:    client:    
    build: .    
    depends_on:      
      - db      
      - api    
    env_file:     
      - .env    
    environment:  
      - PORT=3000 
    ports:     
      - "3000:3000" 
   stdin_open: true 
   tty: true
   volumes:
     - .:/usr/src/app:cached   
   command: ["npm", "run", "client:start"]...
```

在这个 docker-composite . yml 文件中，我们将定义我们所有的服务，这样我们就可以在我们的机器上进行本地开发。由于这是一个单一报告和一个 Dockerfile，我们将从当前目录构建，并将整个报告放入一个 Docker 容器中，以服务于我们的 Full Stack 应用程序。您可以在所有其他服务的回购协议中看到文件的其余部分。

让我们将 Docker 容器保持在最低限度，不要包含要构建到图像中的特定文件。我们通过使用. dockerignore 文件来实现这一点。

```
.git
Dockerfile
docker-compose.yml
.DS_Store
.gitignore
README.md
env.*
sample.env
.env
nginx
db # To prevent storing dev/temporary container data
*.csv
tmp
```

容器中任何您不需要的文件都可以在这里列出来忽略它们。

现在我们已经浏览了文件夹和设置，让我们创建一个。用于所有环境变量的环境文件。

```
# Environment
NODE_ENV=local
TZ=America/Denver
PORT=3000# Auth
JWT_SECRET=ta8fbfdaf2a7869903a644d2827c888b81461a47dd5ab6b6014473eb83c7d4a1X_
SERVICE_TOKEN=ta8fbfdaf2a7869903a644d2827c888b81461a47dd5ab6b6014473eb83c7d4a1# DB
DATABASE_URL=postgres://postgres:postgres@db:5432/postgres
```

在我们的 Api 服务中，我们将使用 JWT (JSON Web Token)进行身份验证，因此我们将创建一个环境变量来存储我们的秘密。我们还将在这里存储我们的本地数据库凭证。

现在，我们可以运行 docker-compose up，构建我们的容器，应用程序应该在本地主机上启动并运行。

```
docker-compose up
```

编译和运行我们的应用程序都由项目根文件夹中的一个 package.json 控制。它可以通过运行 npm 命令来运行所有部署脚本。在 README.md 中，它显示了如何运行迁移和种子。

```
docker-compose run --rm api npm run migrate
docker-compose run --rm api npm run seed
```

让我们来讨论一下我们设置了哪些命令。

*   start —这将在 api 构建完成后启动它
*   API:build——这将把应用程序从类型脚本编译成可部署的 JavaScript
*   API:dev——这将启动一个开发服务器，运行并监视 Typescript 文件
*   api:test —这将在我们的 api 上运行测试
*   api:db-utils —这将运行数据库脚本
*   API:sequel ize——这使我们能够访问 Sequelize CLI
*   客户端:启动—这将启动 React 应用程序开发服务器
*   client:build —这将构建 React 应用程序，为部署做好准备
*   客户端:复制—这将客户端复制到 api 到服务器静态文件中
*   构建—这将构建和编译所有的服务
*   测试—这将测试所有服务
*   迁移—这将运行数据库迁移
*   种子—这将为数据库植入数据
*   heroku-post build——这将为 Heroku 构建我们所有的服务

您可以在 docker-compose.yml 中看到它为本地开发服务使用了哪些 npm 命令。

现在我们已经在本地设置好了，让我们来讨论一下部署。在本教程中，我们将使用 Heroku 和 Github。我们将在 Github 中托管我们的 repo，并在 Heroku 中创建一个与之连接的项目。

让我们设置一个 Procfile，它将在我们推送新代码时运行任何迁移。

```
release: npm run migrate
```

当 Heroku 看到这个文件并发布新代码时，它将运行迁移。

让我们也为 Heroku 添加一些忽略脚本。我们通过创建一个. slugignore 文件来实现这一点。

```
./db
./nginx
.dockerignore
docker-compose.yml
Dockerfile
.env
sample.env
README.md
log
tmp
```

任何你不想在 Heroku 服务的最终版本中出现的东西都在这个文件中。

Api 服务器

这个 Api 将运行 ExpressJs，我们将使用 JWT 进行认证。

```
/** 
 * JWT Authentication 
 */
app.use(  
  expressjwt({    
    algorithms: ['HS256'],    
    secret: process.env.JWT_SECRET,    
    credentialsRequired: false,   
    getToken(req) {     
      if (req.headers.authorization &&       req.headers.authorization.split(' ')[0] === 'Bearer') {       
        return req.headers.authorization.split(' ')[1];      
      } else if (req.query && req.query.token) {        
        return req.query.token;      
      }      
      return null;    
    },  
  })
);
```

这将采用我们的 JWT_SECRET 环境变量并设置我们的身份验证。然后，它将在头或查询字符串中查找令牌。

然后，让我们启动服务器，监听我们的端口环境变量。

```
const server = app.listen(process.env.PORT, () => {  
  console.log(    
    `
     Express Server started. 
     Port: ${app.get('port')}
    `
  );
});
```

[](https://expressjs.com/) [## Express - Node.js web 应用程序框架

### Express 是一个最小且灵活的 Node.js web 应用程序框架，它为 web 和…

expressjs.com](https://expressjs.com/) 

让我们设置一个登录路径，以便用户可以登录。

```
import express, { Request, Response, NextFunction } from 'express';
import { asyncEndpoint, validateSchema } from '../../../../../../middleware';
import joi from '@hapi/joi'; const router = express.Router(); const loginSchema = {  
  path: 'body',  
  schema: joi.object().keys({  
    email: joi.string().required(),  
    password: joi.string().required(),
  }),
}; const login = async (req: Request, res: Response, next: NextFunction) => {  
  const { Users } = req.app.get('db'); 
  const user = await Users.findOne({   
    attributes: ['id', 'accountId', 'email', 'password'],  
    where: {    
      email: req.body.email,  
    },  
  });  if (!user || !user.verifyPassword(req.body.password)) {  
    throw {     
      status: 403,    
      message: 'Email or Password is incorrect',  
    };  
  }  

  req.refreshJWT(user.id, user.accountId);  
  res.end();
}; router.post('/', validateSchema(loginSchema), asyncEndpoint(login)); export default router;
```

在这个文件中，我们将从@hapi/joi 包中引入一些数据验证。我们将验证只允许输入电子邮件和密码。然后我们会检查数据库和密码，看看是否有正确的。

现在我们有了登录系统的端点，我们可以在 React 中编写一个登录组件。

```
import React, { useState } from 'react';
import { useObserver } from 'mobx-react-lite';
import { useAuth } from '../store/Context';
import Button from '@material-ui/core/Button';
import TextField from '@material-ui/core/TextField';
import Dialog from '@material-ui/core/Dialog';
import DialogActions from '@material-ui/core/DialogActions';
import DialogContent from '@material-ui/core/DialogContent';
import DialogContentText from '@material-ui/core/DialogContentText';
import DialogTitle from '@material-ui/core/DialogTitle';
import useSnackbar from 'hooks/useSnackbar';
import Slide from '@material-ui/core/Slide';
import { TransitionProps } from '@material-ui/core/transitions';
import Api from 'services/Api'; const Transition = React.forwardRef(function Transition( 
  props: TransitionProps & { children?: React.ReactElement<any, any> },  
  ref: React.Ref<unknown>) {  
    return <Slide direction="up" ref={ref} {...props} />;
  }); const Login: React.FC = () => {  
  const auth: any = useAuth();  
  const [email, setEmail] = useState('');  
  const [password, setPassword] = useState('');  
  const handleClose = () => {   
    auth.setOpenLoginDialog(false);  
  };  
  const handleRegister = () => {   
    auth.setOpenLoginDialog(false); 
    auth.setOpenRegisterDialog(true); 
  };  
  const handleForgotPassword = () => {
    auth.setOpenLoginDialog(false);     
    auth.setOpenRegisterDialog(false);  
    auth.setOpenForgotPasswordDialog(true); 
  };  
  const handleSubmit = useSnackbar(async (snackbar: any) => {  
    await Api.post('/api/v1/login', {     
      email,     
      password,  
    });    
    window.location.href = '/dashboard'; 
  });  
  return useObserver(() => {  
    return (    
      <Dialog      
        open={auth.openLoginDialog}    
        TransitionComponent={Transition}    
        onClose={handleClose}       
        aria-labelledby="login"      
      >        
        <DialogTitle id="login">Login</DialogTitle>   
          <DialogContent>         
            <DialogContentText>         
              To login to this website, please enter your email address and          
              password here.          
            </DialogContentText>         
            <TextField      
              autoFocus       
              margin="dense"       
              id="email"       
              label="Email Address"       
              type="email"          
              fullWidth   
              value={email}    
              onChange={(e: any) => setEmail(e.target.value)}         />     
            <TextField    
              autoFocus          
              margin="dense"     
              id="password"      
              label="Password"    
              type="password"      
              fullWidth         
              value={password}      
              onChange={(e: any) => setPassword(e.target.value)}          />        
         </DialogContent>    
       <DialogActions>       
         <Button onClick={handleClose} color="secondary">            Cancel         
         </Button>   
         <Button onClick={handleForgotPassword} color="primary">            Forgot Password       
         </Button>      
         <Button onClick={handleRegister} color="primary">            Register        
         </Button>    
         <Button    
           onClick={handleSubmit}  
           color="primary"         
           disabled={!email || !password} 
         >          
           Login     
         </Button>     
       </DialogActions>     
     </Dialog>   
   );  
  });
}; export default Login;
```

这个组件使用一个 Mobx 存储来处理电子邮件和密码的状态。当用户点击登录按钮时，它将启动 handleSubmit 函数并调用我们的后端 Api。

 [## MobX 与反应

### 让您的组件真正与 MobX 反应

mobx-react.js.org](https://mobx-react.js.org/) 

## 开发人员注意

该应用程序中还有一些其他组件，只是订阅测试版访问电子邮件列表的一些基本形式。

您可以在 mono repo 中开发任何您想开发的应用程序。您可以运行任何后端 api。只需替换 api 和客户端文件夹中的内容，您就可以满足您的需求。

请留下评论，说明这是否对您有所帮助，或者您如何解决使用完整堆栈 JavaScript 部署 mono repo 的问题。

[](https://nodejs.org/en/) [## Node.js

### Node.js 是一个基于 Chrome V8 JavaScript 引擎的 JAVAScript 运行时。

nodejs.org](https://nodejs.org/en/) [](https://reactjs.org/) [## 反应-一个用于构建用户界面的 JavaScript 库

### 反应使创建交互式用户界面变得毫无痛苦。为应用程序中的每个状态设计简单的视图，并做出反应…

reactjs.org](https://reactjs.org/)