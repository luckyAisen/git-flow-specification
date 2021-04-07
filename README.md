# 关于 git flow 工作流程 和 git commit message 规范
## git flow 工作流程

### 前言

虽然平时我在工作和学习当中，一直都是使用 `git flow` 规范，但是始终没有形成文档，最近在参与制订公司前端部门的一些开发规范，就把这次的工作和总结形成文档，方便日后复盘。

业界用的比较多的流程主要是：git flow、github flow、和 gitlab flow 这三种，可能有一些公司内部会有自己的 workflow 流程，比如：阿里的**[Aone Flow](https://developer.aliyun.com/article/573549)**、字节跳动的[多种工作流](https://juejin.cn/post/6875874533228838925#heading-5)。

这篇文章，主要是介绍 git flow 工作流，虽然它有一定的缺点，但是也足够满足大部分的中小厂的开发。

关于 github flow 和 gitlab flow 的介绍，请查看参考资料中的第一点和第二点。

先看看一张比较经典的图，下面是 git flow 工作流程图

![git flow](https://img-blog.csdnimg.cn/20210406150232152.png)



### 分支功能

**master**

master 主分支的代码主要是生产环境运行的代码，这分支的代码不能再任何时间任何人修改，要保持 master 分支的稳定性

**develop**

develop 分支的代码始终是项目最新的代码分支，包含了完成新功能的开发和 bug 的修改

**feature**

feature 分支的代码主要是新功能的开发，feature分支都以 develop 分支为基础创建的，分支的命名一般以feature/开头，例如：feature/module、feature/content。

**release**

release 分支的代码主要是预发布环境的代码，提交测试阶段，会以 release 分支代码为基准提测。该分支是由 develop 分支为基准进行创建，分支的命名一般以release/版本号，例如：release/1.0.0、release/1.1.0、release/1.1.1。关于版本号如何定义，请查看[版本号定义](### 开发步骤)

**hotfix**

这个分支的代码主要是解决线上的紧急 bug 的，以 master 分支为基础创建的，命名的规则可以和 feature 一样。当解决完 bug 后，需要合拼到 master 分支和 develop 分支。

### 使用场景设想（开发步骤）

1、老大安排了新的需求，需要做一个用户系统，要求一周内完成并且需要部署上线。你看看当前有没有新需求的分支（多人协作，有可能你的同事已经创建了），如果没有那就创建一个feature分支，命名为"feature/user_module"。如果有当前分支了，那不用创建直接拉取当前这个代码的分支。

```shell
git checkout -b feature/user_module develop
```

2、三天过去了，你做完了这个新的需求，自己也自测通过了，需要提交测试了。首先，先把 feature/user_module 合拼到 develop 分支。合拼完后，删除此 feature/user_module ，在以 develop 分支为基础创建 release 分支，命名为 release/1.0.0 ，最后提交测试。

```shell
# 检出 develop 分支
git checkout develop
# 使用 develop 分支合并功能分支
git merge feature/user_module
# 删除 feature/user_module 功能分支
git branch -d feature/user_module
# 推送远程仓库
git push origin develop
# 创建 release/1.0.0 分支
git checkout -b release/1.0.0 develop
# 推送 release/1.0.0 分支到远程仓库
git push origin release/1.0.0
```

3、测试人员提出了好几个bug，然后在 release/1.0.0 分支上修改，修改完后在此提测。如果还有bug那么也在这个分支上修改提测。一直到没有bug了。

4、一周来到了最后一天晚上了，老大说准备部署生产环境了。这个时候就可以把 release/10.0. 分支合拼到 develop 和 master 分支上了。部署之前，打一个 tag 标签，以便对当前版本的代码存档。

```shell
# 切换到 develop 分支
git checkout develop
# 与远程同步
git pull
# develop 分支合并 release 分支
git merge release/1.0.0
git push origin develop

# 切换到 master 分支
git checkout master
# 与远程同步
git pull
# master 分支合并 release 分支
git merge release/1.0.0
git push origin master

# 在 master 分支上打 tag
git tag -a 1.0.0
# 推送 tag 到远程
git push origin 1.0.0
```

5、做完了这个需求，下面或许还有很多需求，那么可以继续按照上面的流程来。

6、第二天或者第n天后，客户反馈有个 bug 或者有个按钮或者数据出不来，你定位到了问题后，也与团队内的同事或者老大确认过后，需要紧急处理。这个时候以 mastr 为基础创建 hotfix 分支，命名为 hotfix/user_module 分支。修改完这个 bug 后，以 hotfix/user_module 分支为基准，创建 release/1.0.1 分支，创建完后删除 release/1.0.1 分支。并且提交进行测试，如果还是有问题，那么继续在 release/1.0.1  分支上修改，直到没问题后，按照上面的发包流程，进行发包。

### 版本号定义

项目代码release包括三类：

- 大版本(x.0.0)
- 小版本(x.x.0)
- 补丁(x.x.x)

### git flow 的优缺点

**优点：**

- 各分支各司其职，基本上可以覆盖大部分的场景。
- 严格按照流程执行，出现重大事故的情况会大大降低。

**缺点：**

- 过于繁琐，按照这个流程严格执行对团队成员来说按照有难度。
- git flow违反了“短命”分支规则
- git flow也无法在 monorepo中 使用。
- 关于更多的说明，请查看参考资料第四点。



##  git commit message 规范

不知道大家对 git commit message 有没有要规范，之前也有了解过相关的规范，后来因为太懒，不想写这么多 message，所以就没按照规范来。后来我记得有一次，要回去找一个比较复杂方法（这个方法由于种种原因被删除了），而当时提交的时间距离当下来说很远了，差不多有一个月了，因为没有很清晰明确的 commit message，找了很久，后来才找到了这个方法。而且还有一些团队成员不写 commit，会导致回溯问题的时间成本越来越高。

而规范 对 git commit message 是为了以下几个目的：

- 自动生成 CHANGELOG.md
- 识别不重要的提交
- 为浏览提交历史时提供更好的信息

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

subjec t是 commit 目的的简短描述，不超过50个字符。

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



### 工具



### 缺点



## 参考资料

[Understanding the GitHub flow](https://guides.github.com/introduction/flow/index.html)

[Introduction to GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html)

[一文弄懂 Gitflow、Github flow、Gitlab flow 的工作流](https://cloud.tencent.com/developer/article/1646937)

[Please stop recommending Git Flow!](https://georgestocker.com/2020/03/04/please-stop-recommending-git-flow/)

[在阿里，我们如何管理代码分支？](https://developer.aliyun.com/article/573549)

[字节研发设施下的 Git 工作流](https://juejin.cn/post/6875874533228838925#heading-5)



