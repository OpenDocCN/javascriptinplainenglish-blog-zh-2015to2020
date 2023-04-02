# 如何在 Node.js 中发送带有图像附件的电子邮件&如何回应

> 原文：<https://javascript.plainenglish.io/how-to-send-an-email-with-image-attachment-in-node-js-react-657479d49587?source=collection_archive---------1----------------------->

![](img/c313a2117d59f298b65e28cbbb473f61.png)

在现代网络中，具有图像附加特征的接触形式变得越来越流行。你可能会问我为什么需要它？那么，如果您的客户发现了一个 bug 并想向您发送一个截图呢？有道理，对吧？互联网上有很多关于使用 Node.js 和 Nodemailer 发送电子邮件的教程，但是没有一个涵盖使用 node . js 和 node mailer 发送附件。我们将更进一步，使用 Node.js Express 为后端构建带有图像附件的联系人表单，并使用 Redux 为前端做出反应。我们开始吧:

**设置后端**

首先，我们需要建立我们的项目:

```
npm init
```

因此，我假设您已经熟悉了使用 Node.js 和 React 生成项目，所以我不会进一步讨论基本的主题。我们需要安装 Express 框架、正文解析器和其他依赖项。

```
npm install express body-parser nodemailer cors
```

让我们创建一个名为 **index.js** 的主服务器文件:

```
const express = require('express');const bodyParser = require('body-parser');const nodemailer = require('nodemailer');const cors = require('cors');const app = express();const port = 4444;app.use(bodyParser.json({ limit: '10mb', extended: true }))app.use(bodyParser.urlencoded({ limit: '10mb', extended: true }))app.use(cors());app.listen(port, () => {console.log('We are live on port 4444');});app.get('/', (req, res) => {res.send('Welcome to my api');})
```

这是一个由 Express 支持的简单的 Node.js API。在创建它之后，我们需要从节点邮件程序中获得所有的好处，并包括发送带有图像的提交电子邮件的控制器功能。

**创建接触控制器功能**

我们将使用 Gmail 进行身份验证。但是，您可以使用 [Sendgrid 的 API](https://sendgrid.com/) ，这提供了一个更好、更安全的选项。我们在 **Index.js** 中创建它，因为没有任何其他功能。我们只需要这个简单的控制器和 API 路由来发送帖子请求。

```
app.post('/api/v1/contact', (req, res) => {var data = req.body;var smtpTransport = nodemailer.createTransport({service: 'Gmail',port: 465,auth: {user: 'username',pass: 'password'}});var mailOptions = {from: data.email,replyto: data.email,to: 'goshareitio@gmail.com',subject: data.title,html: `<p>${data.email}</p><p>${data.message}</p>`,attachments: [{filename: data.title + ".jpg",contentType:  'image/jpeg',content: new Buffer.from(req.body.image.split("base64,")[1], "base64"),}]};smtpTransport.sendMail(mailOptions,(error, response) => {if (error) {res.status(400).send(error)} else {res.send('Success')}smtpTransport.close();});})
```

在控制器功能中，您将看到带有支持的文件类型的附件字段。这是节点邮件程序附带的一个内置功能。

是时候转向前端了。

**创建 React.js 项目**

我们将使用创建反应应用程序来生成我们的项目。

```
npx create-react-app Contact
```

我猜你熟悉反应，也有一些知识，所以不会进一步解释项目结构。虽然本教程只介绍了现实世界项目的一小部分，但我们将创建可重用组件，因为可以预见，您不仅会将这些可重用组件用于联系人表单，还会将其用于项目中的其他表单，如签到。张贴，评论等等…

**创建可重用的表单组件**

首先，我们将从简单的输入组件开始:

```
import React from ‘react’;export const ProjectInput = ({
 input,
 label,
 type,
 symbol,
 className,
 meta: { touched, error, warning }
}) => (
 <div className=’form-group’>
 <label>{label}</label>
 <div className=’input-group’>
 {symbol &&
 <div className=’input-group-prepend’>
 <div className=’input-group-text’>{symbol}</div>
 </div>
 }
 <input {…input} type={type} className={className} />
 </div>
 {touched &&
 ((error && <div className=’alert alert-danger’>{error}</div>))}
 </div>
 )
```

然后将创建**文本区域**

```
import React from ‘react’;export const ProjectTextArea = ({
 input,
 label,
 type,
 rows,
 className,
 meta: { touched, error, warning }
}) => (
 <div className=’form-group’>
 <label>{label}</label>
 <div className=’input-group’>
 <textarea {…input} type={type} rows={rows} className={className}></textarea>
 </div>
 {touched &&
 ((error && <div className=’alert alert-danger’>{error}</div>))}
 </div>
 )
```

是时候创建这个表单最重要的可重用组件了，一个上传图像的输入。

**创建 ImgFileUpload 组件**

我们将使用**文件阅读器**。它是一个对象，唯一的目的是从`Blob`(因此也是`File`对象中读取数据。

它使用事件传递数据，因为从磁盘读取可能需要时间。

```
import React from ‘react’;export class ImgFileUpload extends React.Component {constructor() {
 super();this.setupReader()this.state = {
 selectedFile: undefined,
 imageBase64: ‘’,
 initialImageBase64: ‘’,
 pending: false,
 status: ‘INIT’,
 }this.onChange = this.onChange.bind(this);
 }setupReader() {
 this.reader = new FileReader();
 this.reader.addEventListener(‘load’, (event) => {
 const { initialImageBase64 } = this.state;
 var { changedImage } = this.props;
 const imageBase64 = event.target.result;
 changedImage(imageBase64);if (initialImageBase64) {
 this.setState({ imageBase64 });
 } else {
 this.setState({ imageBase64, initialImageBase64: imageBase64 });
 }
 });
 }onChange(event) {
 const selectedFile = event.target.files[0];
 var { checkImageState } = this.props;
 if (selectedFile) {
 checkImageState(‘selected’);
 } else {
 checkImageState(‘unselected’);
 }
 if (selectedFile) {
 this.setState({
 selectedFile,
 initialImageBase64: ‘’
 });this.reader.readAsDataURL(selectedFile);
 }
 }render() {return (
 <div className=’img-upload-container’>
 <label className=’img-upload btn’>
 <span className=’upload-text’> Select an image </span>
 <input type=’file’
 accept=’.jpg, .png, .jpeg’
 onChange={this.onChange} />
 </label>
 </div>)
 }
}
```

您可能会找到许多第三方库用于文件选择和上传，但是，建议忽略这些库。其中一些会导致不同的问题，最常见的是因为单一的第三方库而无法创建产品版本。我们在过去使用 Angular 的第三方包时遇到了类似的问题。所以随着你使用的图书馆越来越少，将来的发行也会越来越少。

**建立完整的联系方式**

我们需要将上面创建的所有组件放在一个地方，以获得最终的表单，但在此之前，我们需要安装一些依赖项。

`npm install --save redux react-redux redux-form axios`

然后我们需要创建一个 **index.js** 文件，它将包含我们所有的 Reducer 以及 f**form reducer**

```
import { createStore, compose, combineReducers } from 'redux';import { reducer as formReducer } from 'redux-form';export const init = () => {const reducer = combineReducers({form: formReducer,});const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;const store = createStore(reducer);return store;}
```

然后别忘了实现我们 store inside **App.js** :

```
import React, { Component } from 'react';import { BrowserRouter, Route } from 'react-router-dom';import { Provider } from 'react-redux';import Contact from './components/contact';import './App.css';const store = require('./reducers').init();class App extends Component {render() {return (<Provider store={store}><BrowserRouter><div className='App'><div className='container'><Route exact path='/' component={Contact} /></div></div></BrowserRouter></Provider>);}}export default App;
```

现在，我们可以继续构建我们的最终联系表单。

```
import React from 'react';import { Field, reduxForm } from 'redux-form';import { ProjectInput } from './ProjectInput';import { ProjectTextArea } from './ProjectTextArea';import { ImgFileUpload } from './ImgFileUpload';class ContactForm extends React.Component {state = {imageState: false}render() {const {handleSubmit,pristine,submitting,submitCb,valid,SetImage,loading} = this.props;return (<form onSubmit={handleSubmit(submitCb).bind(this)}onClick={this.resetValues}><Fieldname="email"type="email"label='Email'className='form-control'component={ProjectInput}/><Fieldname="title"type="text"label='Title'className='form-control'component={ProjectInput}/><Fieldname="message"type="text"label='Description'rows='6'className='form-control'component={ProjectTextArea}/><Fieldname="image"label='Image'className='form-control'component={ImgFileUpload}props={{changedImage: (e) => {SetImage(e);this.setState({imageState: true})},checkImageState: (e) => {if (e === 'selected') {this.setState({imageState: true});} else {this.setState({imageState: false});}},}}key={this.props.key}/>{loading ?<buttonclassName='btn btn-primary'type="button"disabled={true}>Sending...</button>:<buttonclassName='btn btn-primary'type="submit"disabled={!valid || pristine || submitting || !this.state.imageState}>Send</button>}</form>)}}const validate = values => {const errors = {};if (!values.email) {errors.email = 'Please enter email!';}if (!values.title) {errors.title = 'Please enter title!';}if (!values.message) {errors.message = 'Please enter message!';}return errors;}export default reduxForm({form: 'ContactForm',validate})(ContactForm)
```

我们使用先前创建的可重用组件，并使用 redux-form 来支持我们的表单。此外，我们为这个表单实现了一些验证，但是，由于这是一个完全不同的主题，我建议为每个字段单独添加更多的验证。除此之外，我们禁用了发送按钮，直到所有字段都被填充。看起来做了很多工作，对吗？作为使我们的联系表单完全起作用的最后一步，我们将为它创建一个容器组件。

```
import React from 'react';import ContactForm from './ContactForm';import { reset } from 'redux-form';import axios from 'axios';import { connect } from "react-redux";import { ToastContainer, toast } from 'react-toastify';class Contact extends React.Component {constructor(props) {super(props);this.state = {errors: [],note: '',loading: false}this.pristine = false;this.Send = this.Send.bind(this);}SetImage = async (image) => {await this.setState({ image });};Send(userData) {let { image } = this.state;userData = { ...userData, image };this.setState({ loading: true });this.sendEmail(userData).then(submited => {toast.success('Email sent successfully');this.props.dispatch(reset('ContactForm'));this.setState({ key: 'cleared' })this.setState({ note: 'Email sent successfully', loading: false });},).catch(errors => {toast.error('Error occured')this.setState({ errors, loading: false })});};sendEmail = async emailData => {console.log(emailData);return axios.post('/api/v1/contact', emailData).then(res => res.data,err => Promise.reject(err.response.data.errors))};render() {const { errors } = this.state;return (<section id='contact'><ToastContainer /><div className='bwm-form'><div className='row'><div className='col-md-5'><h1>Contact Us</h1><ContactFormloading={this.state.loading}submitCb={this.Send}errors={errors}SetImage={this.SetImage}pristine={this.pristine}key={this.state.key}/></div></div></div></section>)}}export default connect(null, null)(Contact);
```

在上面的组件中，我们将数据发送到后端。如果电子邮件发送成功，表格会自动重置。另外，我们使用了一个烤面包机库来显示警告，但是你可以忽略这个库。如您所见，我们使用 Axios 来执行 post 请求并将数据发送到后端。至于 Redux-form 只支持重置基本字段，我们通过使用 contact 表单和 contact 组件中的特殊键，从头开始创建了重置文件输入的功能。

就是这样！希望这篇教程对你有帮助。
你可以在这里下载完整的源代码:
[Github](https://github.com/haykoyaghubyan/react-node-contact)