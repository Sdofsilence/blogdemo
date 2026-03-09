# git workflow

## 博客

* 加上readme.md中的博客文章
* 除了Git Flow，还有哪些工作流程可以选择？<https://juejin.cn/post/7408851018501341238>

## 分类

* git flow
* github flow
* gitlab flow

## 分别相关参考

* git flow :
  * 稀土掘金 git-flow分支模型：从原文到使用<https://juejin.cn/post/7281948788482408460>
  * <https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow>
  * 原文：一个成功的分支模型： <https://nvie.com/posts/a-successful-git-branching-model/>
* github flow :
  * b站视频关于github的使用流程<https://www.bilibili.com/video/BV1pR4y1s79B/?share_source=copy_web&vd_source=6703107ee8dfb713f9eb418442ae02e3>
  * <https://githubflow.github.io/>github自身的github flow介绍 其中使用[github pages](https://pages.github.com/)作为发布
* gitlab flow : ...

## 详细关于git flow的说明

### overview

* 分支：main,develop,feature/,hotfix/,release/
  * 长期分支
    * main:只能用来包括产品代码。你不能直接工作在这个 master 分支上，不直接提交改动到 master 分支上也是很多工作流程的一个共同的规则。
    * develop:是你进行任何新的开发的基础分支。当你开始一个新的功能分支时，它将是_开发_的基础。另外，该分支也汇集所有已经完成的功能，并等待被整合到 master 分支中。
  * 功能分支
    * feature/:特性分支，合并进入develop分支
    * hotfix/:热修复分支，合并进入main和develop分支
    * release/:发布分支，用于测试和版本发布前的修改，合并进入main和develop分支

### 自己的总结

* 三种从dev同步代码到feature的方式(其中说明了rebase,merge,merge --squash的不同)
  * 1.使用feature rebase 到 dev,虽然这是推荐的做法，可以保证线性的feature历史，让feature历史看起来更干净，但是我认为这不优雅，因为它修改了feature的起点历史，但是rebase可以保留作者和作者时间，不同与commit作者和commit时间(git log --format=flutter)
  * 2.使用dev-tmp复制一下dev分支，然后dev-tmp rebase到feature分支，feature分支reset --hard到dev-tmp分支，最后删除dev-tmp分支，也可以保留作者和作者时间，但是偏底层(其他人可能不会用)
  * 3.使用git merge dev --squash命令把dev的内容合并到feature中，不推荐使用，这会导致commit作者和作者时间的历史记录丢失，也就是说无法通过diff工具找到具体的细节代码是谁改写的
* 作为开发者，只需要创建feature分支，在feature上面commit,如果有需要的话，或者有要求需要定期或者每天从dev中同步，使用上面的1.2.两种方式同步代码到feature中，当自己的分支完成后，进行了单元测试后，通过pr或者口头申请进行merge到dev中并删除分支(git flow finish feature/xxx)
* 需要明确的一点就是如何处理hotfix和release合并到dev和main时候的冲突问题,在gitkraken中finish hotfix需要手动处理冲突，但是release确实强制保留release版本的代码，这就是说，如果使用gitkraken这种策略，创建release分支后，需要冻结develop分支，直到release分支合并回main和develop，不然会导致反复的冲突(即dev修改了后又被release强制修改，但是dev又需要改回来，最好是release合并会之前不要或者不允许把feature合并到dev中)。另外一点就是关于如果有冲突，不要手动git merge --continue,而依赖于工具的git finish，因为它有幂等性，已经处理过的过程不会重复(不同的工具自己验证一下)

## 基于主干的workflow(是现代基于ci/cd的推荐方式)

* github flow
* ...
