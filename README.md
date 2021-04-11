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

**同时，需要在项目根目录下新建一个 `.cz-config.js` 的文件，在这个文件中编写格式规范。**

这里可以参考我的配置文件：**[Aisen60/.cz-config.js](https://gist.github.com/Aisen60/9aad3244ada313a74d0941c08eece595)**，我主要配置了以下信息：

- 汉化
- 自定义 type 
- 自定义了 scope(影响范围)
- 覆盖了 messages 提示信息
- 设置了 allowBreakingChanges，只有 type 为 refactor 时，才需要填写 “列举非兼容性重大的变更”
- 指定跳过 footer 步骤

效果图如下：

![效果图](https://img-blog.csdnimg.cn/20210410175622833.png)

如果你想自定义自己团队的规则，可以具体查看 [cz-customizable](https://github.com/leoforfree/cz-customizable#options) 的相关文档

### 校验 commit message

现在规范有了，要如何把规范落地呢，就是说，如果团队成员不遵循规范，如何防止？

思路是：我们可以结合 git hook 在 git commit  时检查是否符合规范。

我们可以使用 [husky](https://github.com/typicode/husky) 工具来更加方便的使用 git hook，使用 [@commitlint/cli](https://github.com/conventional-changelog/commitlint) 和 [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint) 这两个工具来检查 commit message 是否符合规范。

首先，我们先来安装 @commitlint/config-conventional @commitlint/cli 

```shell
# npm 
npm i @commitlint/config-conventional @commitlint/cli -D
# yarn
yarn add @commitlint/config-conventional @commitlint/cli -D
```

安装完成之后，在根目录下，新建一个 `commitlint.config.js` 文件，写入：

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {}
}
```

接下来，我们来安装 husky

```shell
# npm
npm i husky@next -D
# yarn
yarn add husky@next -D
```

在安装的过程中，终端会罗列出这个husky的版本列表，我们选择最新的版本。版本号为：6.0.0

安装完成后，执行 husky 初始化，命令如下：

```shell
# npx
npx husky install
# yarn
yarn husky install
```

初始化完成后，会在项目根目录下，出现一个 `.husky` 文件夹

接着，执行以下命令，添加一个 hook：

```shell
# npx
npx husky add .husky/commit-msg "npx commitlint -e \$GIT_PARAMS"
# yarn
yarn husky add .husky/commit-msg "npx commitlint -e \$GIT_PARAMS"
```

你会看到 .husky 文件夹会多出一个 commit-msg 脚本，脚本会在 git 提交时，检查我们所填写的信息是否符合规范。

我们随便填写一个信息，效果如下：

![校验失败](https://img-blog.csdnimg.cn/20210410185752395.png)



### 自定义校验 commit message

如果你自定义了校验规则，可以使用 commitlint-config-cz @commitlint/cli 这两个工具了，

```shell
# npm
npm i commitlint-config-cz @commitlint/cli -D 
# yarn
yarn add commitlint-config-cz @commitlint/cli -D 
```

新建`commitlint.config.js` ，并且写入：

```js
module.exports = {
  extends: [
    'cz'
  ],
  // 规则在 rules 中填写
  rules: {
  }
};
```

规则的配置，请查看[官方文档](https://commitlint.js.org/#/reference-rules)

备注：虽然上面我自定了规则，但是大致的和官方的一直，该有的 type、scope 都有，所以我在这里可以直接使用官方的检验。



### npm run commit 替换成 git commit 并且 触发 git cz

既然，我们安装了 husky，我们就不在需要输入 `npm run commit` 来提交了，我们只需要在提交一个git hook 钩子，让开发者无感的触发 git cz 。配置如下：

配置可查看[官网](https://github.com/commitizen/cz-cli#husky)

```shell
# npx
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && npx git-cz --hook || true"
# yarn
yarn husky add .husky/prepare-commit-msg "exec < /dev/tty && yarn git-cz --hook || true"
```

效果如下：

![git commit 代替 git cz](https://img-blog.csdnimg.cn/2021041019430057.png)



## 关于 `SourceTree` 工具的配置 和 推送失败问题

如果你不想用命令行想用 SourceTree 这些图形化工具也是可以的，只要 commit message 的格式符合 **Conventional message 格式**就可以了。格式规范请查看规范简介

### 推送失败问题

可能是代码或者 commit message 不符合规范引起的，或者使用了图形化工具没有配置引起的。

如果是代码或者 commit message不符合规范，按照提示和规范修改即可。

### SourceTree 不走 husky git hook 钩子解决方案：

因为  SourceTree  会跳过  Husky git hook 钩子校验，如果你正在使用 SourceTree 图形化工具，找到相关的 git hook 文件，在**头部**添加以下代码：

Mac：

```
PATH="/usr/local/bin:$PATH"
```

Windows：
经测试没发现因为环境所引起的问题



## 总结

规范化了 commit message 的目的是为了团队更好的维护和问题回溯，但是也有一些约束，团队成员需要一定的时间去适应。我觉得前期的成本是可投入的，前期成本的投入远远小于后期成本的投入。



## 参考资料

[优雅的提交你的 Git Commit Messag](https://juejin.cn/post/6844903606815064077)

[commitlint 落地推广](https://www.yuque.com/iyum9i/uur0qi/gg4kt7)

[前端规范化: 使用commitlint:校验你的 git message](https://juejin.cn/post/6856587708995747854#comment)

[自定义规则校验配置](https://commitlint.js.org/#/reference-rules)

[解决Mac下SourceTree pre-commit 被跳过的问题](https://www.jianshu.com/p/7b7b20b35fde)

[Git pre-commit hook failing in GitHub for mac (works on command line)](https://stackoverflow.com/questions/12881975/git-pre-commit-hook-failing-in-github-for-mac-works-on-command-line)

