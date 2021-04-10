# 关于 git flow 工作流程 和 git commit message 规范
## git flow 工作流

关于 git flow 工作流，可以查看我之前写的另外一篇文章：[git flow 工作流程](https://github.com/Aisen60/blog/issues/5)

##  git commit message 规范

不知道大家有没有看过自己提交过的 commit message 信息，我看过我自己的，简直是不忍直视。如下图：

![不忍直视的 git commit message](https://img-blog.csdnimg.cn/20210410143657429.png)

上图是之前做过的一个项目中的 git commit message。让我下定决心去规范化 git commit message 的是：有一次，要去找一个比较复杂方法（这个方法由于种种原因被删除了），而当时提交的时间距离当下来说很远了（差不多有一个月了），因为没有很清晰明确的 commit message，找了很久，后来才找到了这个方法。

有类似于这种情况的发生已经有好几次了，导致我出现这种情况的时候，我总是需要花时间去做这样的工作。于是，我就想了想，能不能借助工具去帮我们自动生成有相关格式的 commit message，规范化 commit message，或者能够体现出 commit 所做的事情就可以了。

查看了相关的资料后，可以使用 [commitizen/cz-cli](https://github.com/commitizen/cz-clii) + [commitizen/cz-conventional-changelog](https://github.com/commitizen/cz-conventional-changelog) + [conventional-changelog/standard-version](https://github.com/conventional-changelog/standard-version) 这几个工具去解决提交信息的格式，和自动生成版本发布日志（changlog）

而对 commit message 规范，是为了以下几个目的：

- 为浏览提交历史时提供更好的信息
- 识别不重要的提交
- 自动生成 CHANGELOG.md

在了解相关的规范之前，我们先来说说 [AngularJS](https://github.com/angular/angular.js)， 它的提交记录很规范很工整，也是被业界同行称赞最多的。AngularJS 用的是 [Angular convention](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines) 这一套规范。而我们下面要讲的是 [Conventional Commits](https://www.conventionalcommits.org/zh-hans/v1.0.0-beta.4/) 这一套规范，这套规范是在 Angular convention 的基础上进行改造和扩展。

### 规范简介

我们先来看看git commit message 的格式

**Conventional message 格式**是这样的

```xml
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

这个格式整体包含了三部分，分别是：header、body、footer

#### header

header 的部分只有一行，包含了三个字段分别是：`type`（必填）、`scope`（选填）和 `subject`（必填）

##### （1）type

type：提交类型，冒号后面要有空格。简单的说有feat，fix，docs等等。**注意，既然是规范，type类型是固定，一定要是规则里面有的。不要自己创造什么task，update，delete**。

一共有下面这11种：

- feat:     		新增产品功能
- fix:               修复 bug
- docs:           文档的变更
- style:           不改变代码功能的变动(如删除空格、格式化、去掉末尾分号等)
- refactor:     重构代码。不包括 bug 修复、功能新增
- perf:            性能优化
- test:             添加、修改测试用例
- build:           构建流程、外部依赖变更，比如升级 npm 包、修改 webpack 配置
- ci:                 修改了 CI 配置、脚本
- chore:          对构建过程或辅助工具和库的更改,不影响源文件、测试用例的其他操作'
- revert:          回滚 commit

具体可查看 [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)。

##### （2）scope 

scope 用于说明 commit 影响的范围，比如全局组件，全局的函数，视项目不同而不同。

##### （3）subject

subject是 commit 目的的简短描述，不超过50个字符。

#### body

body 部分是对本次 commit 的详细描述，可以分成多行。

#### footer

一般在进行重大修改，影响了别的模块的时候，一定要写。注意，该选项和上面要有空行

#### 举个例子

```shell
docs(README.md): 完善 README.md

添加了:
关于 git flow 工作流程
git commit message 规范(未完成)
参考资料
```



### 安装

在安装之前，我们在本地初始化一个仓库和 package.json

```shell
mkdir git-test
cd git-test
git init
yarn init --yes # 或者 npm init --yes
```

在项目根目录下，执行以下命令进行安装：

```shell
# npm
npm install -D commitizen cz-conventional-changelog
# yarn
yarn add commitizen cz-conventional-changelog -D
```

安装完成后，在 package.json 中进行配置：

```json
"scripts": {
  "commit": "git-cz"
},
"config": {
  "commitizen": {
    "path": "node_modules/cz-conventional-changelog"
  }
}
```

配置完成之后，我们执行 commit 命令，发现报错了：

```shell
npm run commit
```

![报错](https://img-blog.csdnimg.cn/20210409172133891.png)

上图提示我们，**没有文件添加到暂存区**，我们先添加一些文件到暂存区，

```shell
git add package.json
```

添加完后，我们再次执行 commit 命令，你会看到如下：

第一步，它会提示我们这次变更的类型，可以进行上下选择：

![提交类型](https://img-blog.csdnimg.cn/20210409172658157.png)

第二步，它会要求我们输入此次变更的影响范围：

![变更的影响范围](https://img-blog.csdnimg.cn/20210410151520553.png)

第三步，它会要求我们输入此次变更的一个简要说明：

![此次变更的一个简要说明](https://img-blog.csdnimg.cn/20210410151744626.png)

第四步，它会要求我们输入一个更加详细的说明（可选填，我这里不填）：

![更加详细的说明](https://img-blog.csdnimg.cn/20210410151818877.png)

第五步，它会询问我们，是否有突破性的变化：

![是否有突破性的变化](https://img-blog.csdnimg.cn/20210410152004624.png)

第六步，它会询问我们，是否需要关闭 issues：

![是否需要关闭 issues](https://img-blog.csdnimg.cn/2021041015205791.png)

我们填完了所有的信息之后，会出下面的提示，证明已经提交成功了：

![完成提示](https://img-blog.csdnimg.cn/20210410152212776.png)

最后，我们看一下这个工具生成的 commit 长什么样子，在项目终端输入：

```shell
git log
```

![commit 最终的样子](https://img-blog.csdnimg.cn/20210410152611680.png)

它会帮我们生成作者信息、提交日期、以及总的提交说明。

再来看一个信息比较多的 commit：

![信息较多的commit](https://img-blog.csdnimg.cn/20210410152928980.png)

### 自定义适配器

上面这一条就是 AngluarJs 的 git commit message 的规范。但是，这一套规范可能不适用于团队或者用的不舒服，比如，自动生成的作息信息和提交日期，这些可以在相关的 git 平台中查看，完全没必要哈。

我们可以借助一个叫做 [cz-customizable](https://github.com/leonardoanalista/cz-customizable) 的工具，制订一套符合自己团队的规范。

在项目终端输入以下命令进行安装：

```shell
# npm
npm i cz-customizable -D
# yarn
yarn add cz-customizable -D
```

安装成功后，需要在 package.json 中修改 config 中的 commitizen 信息：

```json
"config": {
  "commitizen": {
    "path": "node_modules/cz-customizable"
  }
}
```

同时，需要在项目根目录下新建一个 `.cz-config.js` 的文件，在这个文件中编写格式



## 参考资料

[优雅的提交你的 Git Commit Messag](https://juejin.cn/post/6844903606815064077)

