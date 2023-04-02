# 用 JavaScript 动态呈现. docx 文件

> 原文：<https://javascript.plainenglish.io/render-dynamically-a-docx-file-with-javascript-daaed816fcb8?source=collection_archive---------1----------------------->

![](img/2340406a5795a6e231ada048711d1e28.png)

Photo by [Irvan Smith](https://unsplash.com/@mr_vero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大家好，我是斯特凡诺，这是我的第一篇文章！请随意给我一些关于写作或与这篇文章相关的建议🙌🏻
原文[此处](https://dev.to/sintj_/render-dynamically-a-docx-file-with-javascript-3mef) (dev.to)。

# 需求

我正在用 Vue 构建一个 webapp。JS + Nuxt。我的一个客户的 JS:这个应用程序的目的是基于他们制造的产品创建提案，计算价格和一堆其他事情。
最终，完成的提案需要转换成可打印的格式。

# 问题是

每个提案都有不同的基本信息(标题、创建日期等..)和里面不同的产品。每个产品也是不同的，它可能有一些其他产品没有的键/值对。
事实是**我想导出一个看起来总是一样的文件(一个模板)，但用我的提案中的数据渲染**。

# 工具

在网上搜索时，我发现了一个名为 **docxtemplater** 的非常棒的工具，它完全可以完成这项工作。*让我们看看它的作用:*

想象一个. docx (Word，Google Doc 等..)像这样:

```
Hello {name}!
You have all these games: {#gameList}{.} {/gameList}
{#hasXbox}And you have also an XBOX!{/}
```

用 **docxtemplater** 你可以传入一个对象，例如，像这样:

```
let obj = {
 name: `Sam`,
 gameList: [`Metal Gear Solid`, `Crash Bandicoot`, `Final Fantasy 7`],
 hasXbox: false
}
```

渲染后，您将能够下载文档，在这种情况下，将如下所示:

```
Hello Sam!
You have all these games: Metal Gear Solid Crash Bandicoot Final Fantasy 7
```

你注意到了吗？
**数组上的条件和循环是可能的**给你自由构建一个符合你需求的模板。
由于布尔值 *hasxbox* ，整个 Xbox 句子被省略。你也可以在一组对象上循环，这给了你更多的能力。关于整个文档，我建议看一看[的官方网站](https://docxtemplater.readthedocs.io/en/latest/tag_types.html)。

# 设置

正如我之前所说的，我使用的是 Vue，但是下面的方法很容易适应其他环境。

您需要 *npm 安装*一些依赖项:

```
npm install --save docxtemplater jszip@2 jszip-utils file-saver
```

*docxtemplater* 接受 zip，因此 *jszip* 和 *jszip-utils* 对此很有用， *file-saver* 对于保存渲染的文件很有用。设备上的 docx。
*注意:jszip@2 为了防止安装在我的环境中似乎不工作的版本 3+:请随意尝试两者。*

也就是说我像这样把它们导入到组件中:

```
import docxtemplater from ‘docxtemplater’
import JSzip from ‘jszip’
import JSzipUtils from ‘jszip-utils’
import { saveAs } from ‘file-saver’
```

在 html 模板中，我有这个按钮:

```
<button @click=”createDOC()”>Export DOCX</button>
```

然后是方法:

```
methods:{
    loadFile(url,callback){
        JSzipUtils.getBinaryContent(url,callback);
    },

    createDOC(){
        let prev = this.getLoadedPrev
        this.loadFile('/template.docx',function(error,content){
            if (error) { throw error };
            let zip = new JSzip(content);
            let doc = new docxtemplater().loadZip(zip)
            doc.setData(prev)

            try {
                doc.render()
            }

            catch (error) {
                let e = {
                    message: error.message,
                    name: error.name,
                    stack: error.stack,
                    properties: error.properties,
                }
                console.log(JSON.stringify({error: e}));
                // The error thrown here contains additional information when logged with JSON.stringify (it contains a property object).
                throw error;
            }

            let out = doc.getZip().generate({
                type:"blob",
                mimeType: "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
                })
            saveAs(out,`${prev.titolo}.docx`)
        })
    }
}
```

在我的例子中，方法 *loadFile* 将检索。应用程序的静态文件夹中的 docx 模板(所有这一切都发生在客户端，有可能设置所有这一切与节点服务器端)。
jszip 实用程序将压缩。为了实例化一个新的 *docxtemplate* 文档而传递的 docx。

在 *doc.setData(prev)* 中，我用**传递一个对象，该对象包含关于建议书的所有信息(标题、创建日期、产品列表、作者等..)**然后它会尝试渲染 doc。

错误处理后的代码块:

```
let out = doc.getZip().generate({
                type:"blob",
                mimeType: "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
                })
            saveAs(out,`${prev.titolo}.docx`)
        })
```

负责呈现文档的输出。

# 结论

请注意，如果您需要生成一个 PDF 文件，**可以通过**到[这个包](https://www.npmjs.com/package/@nativedocuments/docx-wasm)。对于对 Lambda 函数有信心的人来说，这将是轻而易举的事情。现在我没有这样做的需求，所以我不能用真实的例子来帮助你。

就这些，
干杯！👋🏻