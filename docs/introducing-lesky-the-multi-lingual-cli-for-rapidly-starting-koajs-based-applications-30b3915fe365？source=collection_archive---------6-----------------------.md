# Lesky 简介:用于快速启动基于 KoaJS 的应用程序的多语言 CLI

> 原文：<https://javascript.plainenglish.io/introducing-lesky-the-multi-lingual-cli-for-rapidly-starting-koajs-based-applications-30b3915fe365?source=collection_archive---------6----------------------->

![](img/6348c35af5fcc557c2fa8025b24253c1.png)

TL；DR — [Lesky](https://www.npmjs.com/package/lesky) 是一个相对轻量级的多语言 CLI，只需安装一次(全球范围内)，就可以在任何地方使用，通过“les”调用(不是更多，因为`les`更少)。具体来说，任何文件夹都可以被静态地提供服务并观察变化。此外，任何文件夹都可以使用 CLI 快速初始化为基于 KoaJS 的应用程序。就好像`http-server`和`express-generator`生了一个基于 ES6 的多语言宝宝。这个婴儿知道 42 种不同的语言。

声明:我是 lesky 的作者。

**简介**:

lesky 想要解决的许多问题已经被其他伟大但独立的项目很好地解决了。然而，lesky 的目标是将许多伟大的想法集中在一个包中，以解决以下问题，同时减少用户的工作量:

*   在机器上的任何地方运行它来提供静态内容。有时我需要或更喜欢在我的机器上本地摆弄，而不是在线，因为它通常快得多。
*   支持所有 http 协议(http，https，http2)，而不仅仅是 http(老到 1995！).http3 引起了人们的注意。
*   使用服务器配置的配置文件(这样我就不必再次输入)。
*   立即打开默认浏览器，观察文件更改。
*   默认情况下禁用缓存控制，因为它是一个开发服务器，我希望看到我所做的更改。
*   将服务器的关注点与应用程序、数据库、IO 和 CLI 分开。这样，如果服务器必须用不同版本的 Koa 或者不同的服务器框架来改变，这样做应该很容易。
*   用 KoaJS 样板文件初始化任何工作空间，同时仍然分离服务器和应用程序的关注点。理想情况下，工作空间应该用 eslint、babel、test framework 和其他配置文件进行初始化，这样它就可以准备好了。那些额外的东西给这个项目增加了一点重量，但我认为这是值得的。(我想要类似 express-generator 的东西，但是需要更少的输入，以及更少的基于我个人偏好的重构代码)。
*   它必须是可重用的，并且有可重用的工具。更多这些实用程序可以在`[les-utils](https://www.npmjs.com/package/les-utils)`中找到
*   它必须是多语言的，因为我相信当人们能够用他们的母语使用它时，他们会想得最好。这是最具挑战性的任务，但我认为值得一做。

**安装:**

建议全局安装 lesky，这样您就不必重复安装了:

```
npm i -g lesky
```

如果当前没有可搜索的 npm 全局路径，只需更新 path 环境变量:

```
export PATH=~/.npm-global/bin:$PATH
```

重要说明:即使项目名为“lesky”，命令也将是“les”。项目名称需要“les ”,但已被采用。

**基本用法**:

```
les [path] [options]
```

例如，简单地输入:

```
les
```

这将提供“公共”文件夹中的静态内容(如果您从`/myproject`运行命令，那么`/myproject/public`需要存在；如果它不是简单地创建它或指定您想要服务的路径)

要快速查看应用程序，只需:

```
les -ow # “less ouch”, get it?
```

“o”打开浏览器，“w”监视文件更改。将我的[哑重载脚本](https://raw.githubusercontent.com/richardeschloss/les/master/public/webpack/dumbReloader.slim.js)包含在您希望在更改时自动重载的 html 文件中可能会有所帮助。这远没有热模块重载酷，但对于快速和肮脏的开发工作来说已经足够好了。

**配置 Lesky**

使 lesky 易于配置是关键目标之一，它建立在一个 CLI 设计模式上，这在今天似乎很常见。模式是:首先允许 CLI 参数来自配置*文件*，然后如果参数也*在命令行中传递*，则优先考虑这些参数。这种模式使得“用配置编码”变得非常容易，同时，在需要时在命令行上覆盖配置，而不需要实际更改配置文件。

CLI 选项总是可以在帮助菜单中找到:

```
les -husage: les [path] [options]
options:
 -h, --help Print this help menu
 -i, --init Init lesky in workspace specified by path, defaults to cwd [[cwd]]
 -a, --host Address to use [localhost]
 -p, --port Port to use [8080]
 --proto Protocol to use [http] ({ http, https, http2 })
 --range Port Range (in case port is taken) [8000,9000]
 --sslKey Path to SSL Key
 --sslCert Path to SSL Certificate
 -o, --open Open browser [OS default]
 -w, --watch Watch for file changes in dir [staticDir]-- End of Help ---
```

这意味着此处显示的任何长格式选项都可以在配置文件的 terminal *或*中输入:

```
les --port 8001
```

相当于拥有一个 les 配置文件(`.lesrc`)，其条目为:

```
[{ “port”: 8001 }]
```

然后，简单地输入`les`将在配置的端口 8001 启动服务器。对于配置文件和 CLI，有一些重要的事情需要注意。在配置文件中，结构是服务器配置的数组。这样，如果您想在端口 8001 托管“http”版本，在端口 8002 托管“https”版本，这个配置文件就是您所需要的！无需对代码进行额外的更改:

```
[{
 “host”: “localhost”,
 “proto”: “https”,
 “port”: 8002,
 “sslKey”: “.ssl/server.key”,
 “sslCert”: “.ssl/server.crt”
},{
 “proto”: “http”,
 “port”: 8001
}]
```

重要注意事项和限制:
1。配置文件是指定多个服务器配置的唯一方式。亦即`les --proto http --proto https`可能不会做你认为它会做的事。
2。配置文件需要长格式的选项名称，而不是别名。别名仅在终端中受尊重。原因是，我希望这个文件支持多种语言。别名是固定的，但是长格式的选项名称会根据用户的区域设置而改变。我认为这种设计选择可能会带来更好的开发者体验。

**初始化工作空间**

更有可能的是，需要在您当前工作的文件夹中创建一个基于 lesky/KOA js 的自定义应用程序。除了提供静态内容，您还想做更多的事情。用`les`做这件事很简单:

```
les [path] --init [options]
```

其中 path 指定要初始化的目录(默认为当前工作目录)。在这一步传入的任何选项都将用于自动生成`.lesrc`配置文件，这样您就不必再输入它了。只是消耗它。

**多语言支持**

该工具目前支持 42 种不同的语言。为了让它正常工作，它需要设置“LANG”环境变量，并包含该语言的双字符代码。例如，在我的系统上，它是“en_US”。UTF-8”(char code = = " en ")。Lesky 将尝试解析 2 个字符的代码，并以您的系统语言工作。如果没有设置该变量，很容易指定:

```
LANG=”es” les --ayudar
el uso de: les [path] [options]options:
 -h, --ayuda Imprime este menú de ayuda
 -i, --init Init lesky en el espacio de trabajo especificado por la ruta por defecto cwd [[cwd]]
 -a, --host Dirección [localhost]
 -p, --puerto El puerto a utilizar [8080]
 --proto Protocolo para el uso de [http] ({ http, https, http2 })
 --range Rango de puertos (en caso de que el puerto se toma). Formato de inicio-final [8000,9000]
 --sslKey Ruta de acceso de la Clave SSL
 --sslCert Ruta de acceso a los Certificados SSL
 -o, --abierto Abra el navegador. [OS default]
 -w, --reloj Reloj para los cambios de los archivos en el directorio [staticDir]--- Fin de Ayudar a ---
```

所以现在，懂西班牙语的人可以使用这个工具，而不必用英语思考！所以，这个意思`.lesrc`可以写成这样:

```
[{ “puerto”: 8080 }] // Here, either “port” or “puerto” will work. English is the fall-back
```

或者，可能有人会说法语，想要运行该工具。这也很简单:

```
LANG=”fr” les -ow
Options for locale fr does not exist, will attempt to download
res.statusCode 200
listening at: (proto = http, host = localhost, port = 8020)
navigateur ouvert
servir statique dir public
Serveur tous les configs commencé
```

注意:一些英语仍然混杂在控制台消息中，但是我希望并且理解用户将会看到被翻译的关键消息并且仍然理解英语消息中的数字。我也希望翻译都是正确的。即使有人工智能的帮助，这也不是一件容易的事情。我将计划在另一篇文章中讨论这个冒险。如果我翻译错误，欢迎回复！

小题外话:我最引以为豪的是这个特性，因为它最具挑战性，但我认为它绝对值得一做。这是我对“英语中的代码”这一短语的挑战。我无法想象如果要用非母语来思考，我的开发工作流程会有多难。因此，希望全世界的人都会发现这很有用。

**结论**

Lesky 旨在让您的生活变得更加轻松，包括提供静态内容和初始化新项目。将服务器逻辑从所有其他逻辑中分离出来，是为了在需要更换时，将用户从“服务器锁定”中解放出来。增加了多语言支持，使其易于访问。希望对你有帮助。