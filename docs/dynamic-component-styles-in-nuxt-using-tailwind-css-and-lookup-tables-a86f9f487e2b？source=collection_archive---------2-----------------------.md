# Nuxt 中使用顺风 CSS 和查找表的动态组件样式

> 原文：<https://javascript.plainenglish.io/dynamic-component-styles-in-nuxt-using-tailwind-css-and-lookup-tables-a86f9f487e2b?source=collection_archive---------2----------------------->

![](img/2b886439af4ab3d75d6f1e1ed2abeba8.png)

*本文将假设一些 Nuxt / Vue 的知识*

# 顺风、顺风和顺风

顺风 CSS 是目前前端开发中最热门的话题之一。由 Adam Wathan 创建的实用优先的 CSS 框架，在过去的几年中，它已经从一个辅助项目发展成为一个成功的企业。如果您曾经使用过 Tailwind，您可能会意识到它在构建时利用 PurgeCSS 来删除未使用的样式，并创建一个仅由 web 应用程序中使用的类组成的细长样式表。许多框架现在利用 PurgeCSS 在构建时从产品样式表中删除不必要的内容，您也可以在 Nuxt 中使用它。当在 Nuxt 中创建新项目并选择顺风预设时，PurgeCSS 将被自动安装，尽管您可以使用`nuxt-purgecss`构建模块在任何项目中使用它。

PurgeCSS 是一个很棒的插件，但是，它不能解析或运行 JavaScript，并且在大多数情况下只在构建时使用。因此，如果使用不当，可能会导致开发和生产环境之间出现意外的不一致。

# 用 Tailwind CSS 启动一个新的 Nuxt 项目

让我们开始创建一个全新的 Nuxt 安装，打开您的终端并运行以下命令:

`npx create-nuxt-app <your-project-name>`

为了简单起见，我们将使用默认设置，而不是从 UI 框架中选择 Tailwind CSS。

# 带顺风的 Nuxt 中的动态组件样式

Nuxt (Vue)中组件的一个关键特性是能够传递道具。道具是传递给组件的自定义属性，可用于控制外观和功能。让我们看看如何使用 Tailwind 创建一个简单的按钮组件，它接受两种颜色，“主要”和“次要”:

```
<template>
    <button 
        class="px-4 py-2 text-center transition-colors duration-300 ease-in-out border border-solid rounded shadow-md"
        :class="{ 
            'bg-blue-800 text-white border-blue-800 hover:bg-transparent hover:text-blue-800 hover:border-blue-800' : color == 'primary',
            'bg-transparent text-blue-800 border-blue-800 hover:bg-blue-800 hover:text-white hover:border-blue-800' : color == 'secondary'
        }"
    >
        <slot />
    </button>
</template><script>
    export default {
        name: 'component--button', props: {
            color: {
                required: false,
                type: String,
                default: 'primary',
                validator: value => {
                    return ['primary', 'secondary'].includes(value)
                }
            }
        }
    }
</script>
```

因此，我们的按钮组件接受 2 种动态颜色，“主要”和“次要”，正如我们所设定的那样，然而，即使在这个简单的组件中，也很容易看到这些动态样式如何在更复杂的组件中失控。我们也有一个颜色道具验证器，我们必须手动与模板中的动态样式保持同步。

# 提取样式并保持验证器与查找表同步

如果您没有听说过查找表，查找表是一个简单的键-值对对象，我们可以用它来匹配键和数据。我们可以利用查找表来提取动态样式，并确保我们的验证器始终与这些动态样式保持同步。

对于这个例子，我们将在项目的根目录下创建一个名为`validators`的新文件夹来存储我们的查找表，尽管如果愿意，也可以使用相同的技术来利用组件文件中的查找表。一旦你创建了一个名为`validators`的新文件夹，在里面创建一个名为`Button.js`的新文件。在`Button.js`中，我们将导出一个名为`ButtonColors`的`const`，一个包含动态样式的键值对的查找表，如下所示:

```
export const ButtonColors = {
    'primary': 'bg-blue-800 text-white border-blue-800 hover:bg-transparent hover:text-blue-800 hover:border-blue-800',
    'secondary': 'bg-transparent text-blue-800 border-blue-800 hover:bg-blue-800 hover:text-white hover:border-blue-800'
}
```

现在我们已经将动态样式提取到一个查找表中，我们需要对组件进行一些更改，首先，在开始的脚本标签下，我们需要导入我们的`ButtonColors const`:

```
<script>
import { ButtonColors } from '~/validators/Button'export default {/**/}
</script>
```

接下来在我们的`color` props 验证器中，用来自`ButtonColors`查找表的一个键数组替换这个数组:

```
/**/
validator: (value) => {
    return Object.keys(ButtonColors).includes(value)
},
/**/
```

现在，我们可以创建一个计算属性来处理组件模板中的动态类:

```
<script>
/**/
export default {
    /**/
    computed: {
        buttonColor(){
            return ButtonColors[this.color]
        },
    }
}
</script>
```

然后，我们可以用新的计算属性替换模板中的所有动态类:

```
<template>
    <button 
        class="px-4 py-2 text-center transition-colors duration-300 ease-in-out border border-solid rounded shadow-md"
        :class="[buttonColor]"
    >
        <slot />
    </button>
</template>
```

总之，这应该给我们一个组件模板，如下所示:

```
<template>
    <button 
        class="px-4 py-2 text-center transition-colors duration-300 ease-in-out border border-solid rounded shadow-md"
        :class="[buttonColor]"
    >
        <slot />
    </button>
</template><script>
    import { ButtonColors } from '~/validators/Button' export default {
        name: 'component--button', props: {
            color: {
                required: false,
                type: String,
                default: 'primary',
                validator: value => {
                    return Object.keys(ButtonColors).includes(value)
                }
            }
        }, computed: {
            buttonColor() {
                return ButtonColors[this.color]
            },
        }
    }
</script>
```

一切看起来都很好，我们的动态样式被提取，我们的验证器将自动与我们添加的任何新的动态样式保持同步，然而不幸的是，目前我们的组件仍然不会像生产中预期的那样进行样式化。幸运的是，有一个非常简单的修复，从项目的根打开`tailwind.config.js`并在`purge`对象中，找到`content`数组并添加`'validators/*.js'`，这将告诉 PurgeCSS 检查我们的 validators 文件夹中的样式，我们最终的`purge`对象应该看起来像这样:

```
purge: {
    // Learn more on https://tailwindcss.com/docs/controlling-file-size/#removing-unused-css
    enabled: process.env.NODE_ENV === 'production',
    content: [
        'components/**/*.vue',
        'layouts/**/*.vue',
        'pages/**/*.vue',
        'plugins/**/*.js',
        'validators/*.js',
        'nuxt.config.js'
    ]
}
```

# 结论

希望您发现这是一个在 Nuxt、Tailwind 和 PurgeCSS 中清理动态类的有用练习。

如果你觉得这篇文章有用，请在[媒体](https://medium.com/@wearethreebears)、[开发人员](https://dev.to/wearethreebears)和/或[推特](https://twitter.com/wearethreebears)上关注我。