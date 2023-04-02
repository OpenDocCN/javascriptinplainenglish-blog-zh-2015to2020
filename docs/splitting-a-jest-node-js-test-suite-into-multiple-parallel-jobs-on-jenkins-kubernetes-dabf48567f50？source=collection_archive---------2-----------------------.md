# 用 Jenkins 和 Kubernetes 将您的 Jest 测试套件分成多个并行作业

> 原文：<https://javascript.plainenglish.io/splitting-a-jest-node-js-test-suite-into-multiple-parallel-jobs-on-jenkins-kubernetes-dabf48567f50?source=collection_archive---------2----------------------->

![](img/91c07d6963be600ebfba3869dd761076.png)

Photo by [Christian Englmeier](https://unsplash.com/@christianem?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

速度是一个让人渴望的目标……不知何故，当你走在去某个地方的高速公路上时，更快地到达那里是你的目标，实现这个目标会让你比同一条高速公路上的其他人感觉“更好”。

我热爱速度，并不断努力在房间里跑得最快…但有些问题最好通过将赛道切成两半来解决。如果你能承担额外的工作，同时做多件事可以让你更上一层楼。

# 什么是可以接受的？

我记得有一次我在读一篇非常不值得回忆的博客帖子，但是我记得的是，帖子中说他们公司花了一整天**的时间**来运行他们的端到端测试套件，并且等待这些测试很麻烦。我坐在这里阅读和思考…嗯，是的，一天是一个非常长的反馈循环。虽然这个轶事不是 100%相关，因为这个博客是关于服务器单元/集成测试而不是 e2e 测试的，但是它很好地传达了测试套件经常在缺乏关注的情况下增长。

人们增加保险是因为增加保险很容易。维护测试套件周围的区域经常被忽视或者不习惯。持续集成管道周转时间的微小增长可以为开发团队节省大量时间，并加快发布新功能和关键错误修复的部署周期。这两样东西是无价的。

不管节省时间还是潜在的好处，作为开发人员，我们应该追求工作的质量和效率。我们不能让花园里的杂草失去控制。永远记得问自己这样一个问题……如果某件事让你等得很麻烦，那真的可以接受吗？

# 我们的世界

在 Galley Solutions，我们最近完成了一些出色的工作，将我们的 e2e 测试运行时间从大约 35 分钟减少到大约 13 分钟，节省了大量时间！

虽然这很棒，但这并不意味着我们的 CI 渠道在整体上节省了大量时间。我们有一个 Node.js API 测试套件，运行约 30 分钟，在 e2e 优化后，API 测试成为新的速率限制步骤。对于准备对给定的特性或 PR 低头的开发人员来说，及时收到分支测试的反馈是至关重要的。我们决定是时候利用我们的基础设施中的一些现有优势，并创建多个并行执行阶段，每个阶段执行测试套件的一个给定部分。

# 成功的步骤

我们需要执行以下三个步骤。

1.  我们需要能够运行一个命令，对他们自己的数据库执行一组特定的测试文件。
2.  我们需要一些代码，这些代码将接受一个常量`NUM_PARALLEL_STEPS`并生成一组文件，每个文件都包含要在给定步骤中运行的文件。
3.  最后，我们需要一些基于`NUM_PARALLEL_STEPS`常量动态生成每个 API 阶段的代码，这些代码将使用上面步骤 2 中的文件调用步骤 1 中的命令。

# 假设和依赖性

好的，在我们开始之前，让我们陈述一些我们正在处理的依赖和约束…

1.  我们的大多数测试都是集成测试，所以需要一个数据库
2.  在 Kubernetes 上运行的 Jenkins(或能够执行工人作业)
3.  脚本化的 Jenkinsfile 语法(不是必需的，但这是代码示例的语法)
4.  带有 Typescript 和/或的 Jest 和 Node.js。js 文件
5.  Sequelize/Postgres 作为 ORM/DB(不是必需的，但这是示例所针对的)

我想我们准备好了。

# 第一步。测试命令

我们将首先设置我们的 sequelize 配置来读取一个`DATABASE_URL`环境变量。

```
module.exports = {
  "test-parallel": {
    use_env_variable: "DATABASE_URL",
    dialect: "postgres",
    logging: false
  }
}
```

上例中的`test-parallel`指的是我们的`NODE_ENV`。相应的`sequelize.ts`初始化文件看起来会像这样…

```
import { Sequelize } from "sequelize";const env = process.env.NODE_ENV!;const { use_env_variable, ...config } = require("./config")[env];const sequelize = new Sequelize(process.env[use_env_variable]!, config)export default sequelize;
```

这可能看起来有点过分，但我们通常在配置中有更多的环境。这里真正重要的是`new Sequelize("DATABASE_URL", {dialect: "postgres", logging: false})`被实例化，并在第一个参数中被告知寻找`DATABASE_URL`。

在没有实际数据库的情况下进行顺序配置对我们没有好处。当我们构建 postgres 容器时，我们没有数据库，需要一些东西来处理数据库的创建和迁移。shell 脚本就可以了。

`db_rebuild.sh`看起来会像这样…

```
NODE_ENV=test-parallel yarn sequelize db:create 2> /dev/null || :m=`NODE_ENV=test-parallel yarn sequelize db:migrate:status | grep ^down | wc -l`if [ $m -gt 0 ]
then
    echo RECREATING DATABASE
    NODE_ENV=test-parallel yarn sequelize db:drop
    NODE_ENV=test-parallel yarn sequelize db:create
  else
    echo MIGRATING DATABASE
  fi
  NODE_ENV=test-parallel NODE_PATH=./src yarn sequelize db:migrate
else
  echo DATABASE IS UP TO DATE
fi
```

*功劳归于* [*凯尔·轩尼诗*](https://medium.com/u/c479a67cd754?source=post_page-----dabf48567f50--------------------------------) *以上为*

第一行将创建数据库或给我们一些控制台输出，第二个命令将询问有多少迁移没有被应用。如果尚未应用的迁移数大于 0，我们将删除并重新创建数据库，然后执行迁移。

现在我们有了一个已经存在并完全迁移的数据库。接下来，我们将需要处理我们的测试命令，将这些部分组合在一起。

在我们的 package.json 中(我们使用 yarn)，在脚本部分我们需要一个命令。我们希望在 ci 中这样做，该命令的不同之处在于它执行测试时会查看文件的输出，因此我们可以将其命名为类似于`test:ci:parallel`的名称。这个命令需要调用我们的 db 脚本并执行我们的测试命令。

```
"scripts": {
  "test:ci:parallel": "./<PATH_TO>/dbrebuild.sh && NODE_ENV=test-parallel yarn run jest --ci --runInBand"
}
```

现在我们可以测试这个了！

让我们用测试套件中的两个测试名创建一个文件。

`touch my_tests && vi my_tests`

粘贴文件名并保存，然后

```
export DATABASE_URL=postgres://<USER_NAME>:<PASSWORD>[@localhost](http://twitter.com/localhost):5432/<DB_NAME>
```

用随机的东西替换 user_name、password 和 db_name，但最终这个变量将是在给定 Node.js 容器的 env 上定义的变量。

最后，让我们使用 cat…调用带有文件输出的命令

`yarn test:ci:parallel $(cat my_tests)`

Jest 应该针对上面给出的任何`DB_NAME`执行两个测试(基于模式匹配)。

问题？请在下面留下回答，我们可以进行故障排除。

# 第二步。生成测试清单文件

我们第二步的目标是创建一段代码，这段代码将采用`NUM_PARALLEL_STEPS`，并将我们的测试套件分成与并行步骤一样多的文件。我们可以在 Jenkinsfile 中使用 groovy 做到这一点，但是在探索了一点之后，我发现走不同的路线更容易……这意味着它即将变得脆弱。

**注意:**为了简化事情，我们可以采用一个更复杂的命令，并将所需的行为片段分解成更小的块。将所有较小的命令串在一起将得到我们想要的全局输出。

## 用 Globbing 列出所有文件

让我们从简单地列出所有测试文件开始，为此我们利用 [globbing](https://linuxhint.com/bash_globbing_tutorial/) 。

如果我们在项目根目录中，并且我们所有的代码和测试都在`./src` 子目录中，我们可以像这样…

**苹果电脑:**

`./ls src/**/*.spec.js src/**/*.spec.ts`

**Debian:** (我们使用的 jenkins 映像的 linux 版本)

`find ./src -printf ‘%f\n’ | grep .spec`

## 将输出分割成文件

太好了！现在我们有了一个测试文件字符串的列表，我们如何将一定数量的字符串放入文件清单中呢？假设我的列表中有 10 个文件字符串，我希望一个文件中有 5 个，另一个文件中有 5 个。

我们可以利用`split`和这些选项……
`split -a 1 -d -l 5 — api_`

`-a`选项指定附加后缀的长度，`-d`指定数字后缀，`-l`是每个文件的行数，`api_`是我们要给每个文件的前缀。所以这个命令会把一个 10 行的文件分成两个 5 行的文件，分别叫做`api_0`和`api_1`。

您所要做的就是将第一个命令的输出通过管道传递给 split 命令。

**MacOS:**

`./ls src/**/*.spec.js src/**/*.spec.ts | split -a 1 -d -l 5 — api_`

**Debian:** (我们使用的 jenkins 映像的 linux 版本)

`find ./src -printf ‘%f\n’ | grep .spec | split -a 1 -d -l 5 — api_`

完美！

## 计算每个清单中的文件数量

最后一部分是在 split 命令中为`-l`动态创建数字。随着我们测试套件的增长，每个文件的测试数量需要增长，因为`NUM_PARALLEL_STEPS`是不变的。

如果我们使用`wc -l`作为`NUM_TESTS`从测试中获取行数，然后用`NUM_TESTS`除以`NUM_PARALLEL_STEPS + 1`(处理奇数情况)，我们将得到每个文件的行数。

**MacOS:**

`echo $(($(./ls src/**/*.spec.js src/**/*.spec.ts | wc -l)/3+1))`

Debian: (我们使用的 jenkins 映像的 linux 版本)

`echo $(($(find ./src -printf ‘%f\n’ | grep .spec | wc -l)/3+1))`

恭喜你。这是第二步。到最后一个…

# 第三步。平行詹金斯阶段

这就是我们把所有东西都集中在一起的地方。詹金斯档案。游戏时间。没有囚犯。自由。眼睛盯着奖品…

![](img/c1f401e114524f7aadb30709181c63af.png)

Photo by [Japheth Mast](https://unsplash.com/@japhethmast?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我们的构建阶段，我们将使用步骤 2 中的命令，因为当我们生成并运行并行阶段时，我们需要测试清单文件存在。我们可以利用 for 循环和我们的`NUM_PARALLEL_STEPS`常量在一个`tests`对象中添加阶段，我们将在 Jenkinsfile 中用`parallel`命令调用这个对象。

走吧。

## 确保数据库设置正确

```
containerTemplate(
            name: 'api-db',
            image: 'postgres:11.2-alpine',                 ttyEnabled: true,
            envVars: [
                envVar(key: 'POSTGRES_USER', value: '<USER_NAME>'),
                envVar(key: 'POSTGRES_DB', value: 'default'),
                envVar(key: 'POSTGRES_PASSWORD', value: '<PASSWORD>')
            ],)
```

在上面的`api-db`容器中，让我们为`USER_NAME`和`PASSWORD.`指定一个环境变量。你可以在那里放一个随机的名字。

我们将使用的另一个容器是`node`容器。

## 构建阶段

假设我们的构建阶段看起来像…

```
stage('Build Dependencies'){
                container('node') {
                    try {
                        sh "yarn install --frozen-lockfile"
                        sh "cd api && yarn build:test"
                    } catch (Exception e) {
                        notifySomethingErrored()
                        throw e
                    }
                }
        }
```

我们安装我们的依赖项，然后从我们的`./src`目录开始构建。如果发生错误，我们会通知。

**注意:**在我们的例子中，我们针对我们的构建目录运行测试，所以这些命令的其余部分将只引用`.spec.js`文件，但是您可以轻松地针对`./src`运行测试，并在您的命令中包含`.spec.ts`。

在我们运行完`yarn build:test`之后，我们想要计算我们的`numAPISpecsPerStage`。为此，我们可以从步骤 2 中提取命令，并将 STDOUT 输出到`numAPISpecsPerStage`。

```
def numAPISpecsPerStage = sh(
        script: "cd packages/api && echo \$((\$(find ./build -printf '%f\n' | grep .spec.js\$ | wc -l)/${NUM_PARALLEL_STEPS}+1))",
        returnStdout: true
).trim()
```

**注意:**在 groovy 中执行 bash 命令时，如果我们希望 bash 解释任何`$`字符，我们需要对其进行转义，否则，groovy 会试图将它们解释为变量。

接下来，我们将使用上面的变量来构建我们的清单文件

```
sh "cd packages/api && find ./build -printf '%f\n' | grep .spec.js\$ | split -a 1 -d -l ${numAPISpecsPerStage} - api_"
```

最后，让我们调用一个随机函数，`buildApiStages()`，我们还没有写。

将这些插入到`build`级产生，

```
stage('Build Dependencies'){
                container('node') {
                    try {
                        sh "yarn install --frozen-lockfile"
                        sh "cd api && yarn build:test"
                        def numAPISpecsPerStage = sh(
                            script: "cd packages/api && echo \$((\$(find ./build -printf '%f\n' | grep .spec.js\$ | wc -l)/${NUM_PARALLEL_API_STAGES}+1))",
                            returnStdout: true
                         ).trim()
                        sh "cd packages/api && find ./build -printf '%f\n' | grep .spec.js\$ | split -a 1 -d -l ${numAPISpecsPerStage} - api_"
                        buildAPIStages()
                    } catch (Exception e) {
                        notifySomethingErrored()
                        throw e
                    }
                }
        }
```

是的，我们完全可以将我们的文件列表输出提取到一个变量中，我们用管道`echo`和`wc`和`split`进行清理。

## API 阶段

接下来，我们可以充实`buildApiStages()`函数。在文件的顶部附近，或者任何你想定义函数的地方，让我们复制当前的 API 测试阶段代码。

```
"api": {
            timeout(time: 40, unit: 'MINUTES') {
                container('node') {
                        try {
                            sh "cd api && yarn test:ci"
                        } catch (Exception e) {
                            notifySomethingErrored()
                            throw e
                        }
                }
            }
        }
```

当前代码执行测试或通知失败。让我们修改一下，首先将我们关心的环境变量暴露给容器。

在`try`的正上方，我们将利用`withEnv`包装器。

```
withEnv([“DATABASE_URL=$DATABASE_URL”, “NODE_ENV=test-parallel”, “NODE_PATH=./build”]){
 …code
}
```

我们公开一个`$DATABASE_URL`并将其设置为一个我们还没有构建的变量。然后，我们将我们的 env 设置为`test-parallel`，并将我们的`NODE_PATH`指向`./build`(如果您正在运行源代码，您可以指定`./src`)。

最后，我们可以在步骤 1 中添加我们的测试命令，对于我们想要`cat`出来的文件，让我们使用一个我们还没有构建的变量，比如`UNIT_TEST_FILE_NAME`。

```
sh "cd packages/api && yarn test:ci:parallel \$(cat ${UNIT_TEST_FILE_NAME})"
```

这给了我们一个 API 阶段，看起来像…

```
"api": {
          timeout(time: 40, unit: 'MINUTES') {
            container('node') {
              withEnv(["DATABASE_URL=$DATABASE_URL", "NODE_ENV=test-parallel", "NODE_PATH=./build"]){
                try {
                  sh "cd api && yarn test:ci:parallel \$(cat ${UNIT_TEST_FILE_NAME})"
                } catch (Exception e) {
                  notifySomethingErrored()
                  throw e
                }
              }
            }
          }
        }
```

## 多个 API 阶段

如果你注意到，`“api”`只是一个键，我们把那个键设置成一个对象(我们的 stage)。为此，我们可以实例化一个`tests`映射，然后利用一个 For 循环来设置几个不同的键，使其等于多个阶段。

`tests = [:]`

现在让我们定义我们的变量`NUM_PARALLEL_API_STAGES=2`并开始我们的函数和循环。

```
def buildAPIStages(){
 for(int i = 0; i<NUM_PARALLEL_API_STAGES; i++) {
  ...code
 }
```

此外，我们还需要在循环的上下文中定义一些变量。

`def UNIT_TEST_FILE_NAME = “api_${i}”`

我们将把它作为`tests`地图中的关键。该变量与`split`命令的文件名相关联。`split`给了我们`api_0`、`api_1`等…所以每个阶段都会根据 loops 索引调用一个特定的清单文件。

```
def DATABASE_URL = “postgres://<USER_NAME>:<PASSWORD>[@localhost](http://twitter.com/localhost):5432/$UNIT_TEST_FILE_NAME”`
```

填写`<PASSWORD>`和`<USER_NAME>`对应于`api_db`后缀容器上定义的变量。

现在，对于最后一部分，让我们将测试映射中的键设置为 api stage 对象。给我们最后一个功能…

```
def buildAPIStages(){
    for(int i = 0; i<NUM_PARALLEL_API_STAGES; i++) {
        def UNIT_TEST_FILE_NAME = "api_${i}"
        def DATABASE_URL = "postgres://<USER_NAME>:<PASSWORD>@localhost:5432/$UNIT_TEST_FILE_NAME"
        tests["${UNIT_TEST_FILE_NAME}"] = {
            timeout(time: 40, unit: 'MINUTES') {
                container('node') {
                    withEnv(["DATABASE_URL=$DATABASE_URL", "NODE_ENV=test-parallel", "NODE_PATH=./build"]){
                        try {
                            sh "cd api && yarn test:ci:parallel \$(cat ${UNIT_TEST_FILE_NAME})"
                        } catch (Exception e) {
                            notifySomethingErrored()
                            throw e
                        }
                    }
                }
            }
        }
    }
}
```

我们很快就要完成了。在`build`阶段，我们建立了我们的文件并建立了我们的动态阶段，现在我们只需要告诉 Jenkins 并行执行它们。

有了这个功能…

```
def doParallelStages(){
 parallel tests
}
```

而这个阶段，放置在`build`阶段之后…

```
stage(‘Run Tests’){
 doParallelStages()
}
```

# 鳍状物

我们的世界现在是并行的。问题、议题、如何做得更好的想法，在回复中给我写一行字，我将很乐意向您学习或提供帮助。

这类工作真正酷的地方在于，您真的为整个组织节省了这么多时间。它让工作生活(也许还有个人生活？？)对你的同事和队友来说更容易、更有效率。有时，这可能看起来并不紧迫或必要，但当您花时间提高组织的效率时，开发人员的幸福感和工作效率将飙升。

结果，我们的总测试时间从大约 30 分钟缩短到 20 分钟。对我来说，这并不真实，需要一些调查。我本以为测试时间会是 15 分钟。我们正在共享资源，因此在同一节点上运行更多的并行作业可能会对性能造成轻微影响。但是，我会接受 10 分钟的节省:-)。

我在 CI/CD 方面有更多的工作要做，所以请继续关注未来一些关于创建最佳 CI/CD 管道的新技术和方法的帖子。

在那之前，干杯，快乐发展。