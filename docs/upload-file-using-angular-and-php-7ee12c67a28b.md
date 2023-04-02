# 如何用 Angular 和 PHP 上传文件

> 原文：<https://javascript.plainenglish.io/upload-file-using-angular-and-php-7ee12c67a28b?source=collection_archive---------2----------------------->

![](img/b4f898c89d9eb807d369404fc68b939d.png)

如何上传文件？。任何编程开发人员的初学者都会想到这个问题。为什么？因为向服务器发送数据(意味着键值对或表单数据)不同于发送 blob(图像、文件)对象。

让我们来看看**如何简单的用 **PHP** 用 **Angular** 上传**一个**文件**。这没有你想象的那么难。但是当我是初学者时，我觉得上传文件太难了。每当我得到一个文件上传任务，我觉得为什么它不适合我。我有时会感到沮丧。但是现在我能够轻松地完成文件上传任务。

如果你从上面看，文件上传过程有三个阶段**。它们是非常容易的阶段。**

1.  **创建一个文件类型为普通 HTML 的**输入字段**。**
2.  **用 JavaScript 创建一个 **FormData** 对象并附加所选文件。然后使用 **HTTP** 客户端传递表单数据。**
3.  **使用 **PHP** 代码捕获文件(通常的 PHP 文件捕获代码)。**

**让我们在一个循序渐进的例子中看看如何使用 Angular 和 PHP 上传文件。**

# ****1。创建角度项目****

**使用下面的命令创建一个角度项目。**

```
ng new erp
```

**将需要的模块添加到 **app.module.ts** 文件中。**

**对于文件输入类型，我们需要以下模块。**

```
FormsModule
ReactiveFormsModule
```

**为了向服务器发送数据，我们需要下面的模块。**

```
HttpClientModule
```

**因此，在您的 **app.module.ts** 文件中添加以下代码。**

****我用的是 Angular Material UI 框架。这是一个可选步骤。****

**在终端/命令提示符下使用下面的命令添加角度材质。**

```
ng add [@angular/material](http://twitter.com/angular/material)
```

**因此，在您的 **app.module.ts** 文件中添加以下代码。**

# ****2。创建文件输入****

**打开**app.component.html**文件，创建如下所示的文件输入类型。**

**这里我创建了一个 **formControl** 元素来引用文件输入。**

****fileChange()** 函数用于检测文件大小和扩展名。如果需要，您可以在选择文件时添加限制。**

**创建一个按钮来上传选定的文件。**

****uploadFile()** 函数用于将文件附加到 formData 对象，并使用 HTTP 请求发送。**

# ****3。在类型脚本中创建表单数据****

**首先，在 **app.component.ts** 文件中为输入字段创建一个 formControl 元素。**

```
file=new FormControl('')
```

**然后创建一个变量来保存选定的文件信息。**

```
file_data:any=''
```

**现在创建一个 fileChange()函数，如下所示。**

**在上面的代码中**

1.  **使用 **fileList.length** 检查文件是否被选中**
2.  **然后我们可以使用**文件名、文件大小、文件类型**来获取文件信息**
3.  **我们可以检查文件大小。这里我使用这段代码将文件大小限制为 4 MB。【file.size/1048576】**<= 4**。您可以根据需要对此进行修改。**
4.  **然后创建一个 formData 对象，并使用 **append()** 函数进行附加。**
5.  **还可以使用 append()函数以键-值对格式传递附加信息。**

# ****4。使用 HTTP** 发送文件**

**要将文件发送到服务器，首先，我们需要在 **app.component.ts** 文件中使用构造函数创建一个 HTTP 的引用。**

**然后用服务器的 IP 地址**创建一个变量。****

**现在调用 HTTP 请求的 **post()** 方法，并像下面这样传递 formData 对象。**

# ****5。使用 PHP** 捕捉文件**

**客户端代码结束了。现在我们需要使用 PHP 服务器捕捉上传的文件。为此，您需要在您的机器或云服务器上安装 PHP。**

**在服务器上创建一个文件夹来保存上传的文件。在这里，我创建了 **docs** 文件夹来存储文件。**

**我有一个冗长的代码捕捉和存储文件。我用结构来避免许多问题。你可以在代码中找到解释。**

**希望你喜欢这个教程。**

**即使复制粘贴以上所有代码，也能正常工作。**

****样本输出:****

**![](img/87442779a3b354cbccc09760f4cbac74.png)**

****完整源代码链接:**[https://github . com/bharathirajatut/angular-examples/tree/master/file-upload-example](https://github.com/bharathirajatut/angular-examples/tree/master/file-upload-example)**

## ****常见错误****

**在上传一个文件时，你将会面临下面的错误**，即使你的代码中没有错误。****

```
**Warning**: move_uploaded_file(docs/245f99482bb6035-377112-avpng.png): failed to open stream: Permission denied in
```

**为什么因为它与文件夹**权限**和 **OS** 有关。通常这个错误只会在 Mac 和 Linux 上出现**。不在 windows 中。****

## ****解决方案****

**在 **Ubuntu Linux** 上，使用以下命令允许文件上传。**

```
sudo chown www-data folder_name
sudo chmod 0755 -R folder_name
```

****在 Mac 上**，右击文件夹- >获取信息- >选择阅读&写给所有人。**