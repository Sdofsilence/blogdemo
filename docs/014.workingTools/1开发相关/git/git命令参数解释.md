# git 命令的参数解释

哪些参数是必须的，参数顺序和含义,详细见[https://git-scm.com/docs](https://git-scm.com/docs)

## 可能会困惑的命令

### project

* git init
  * [-b | --initial-branch=\<name\>] : 指定默认创建的分支名
  * \<directory\> : 默认是当前文件夹下，如果指定路径但是不存在，自动创建目录
  * --bare : 裸仓库无工作目录和暂存区无法在其中工作，用于搭建git服务器的仓库
* git clone
  * \<repository\> [\<directory\>] : 仓库地址 和 本地目录名(不填则为项目名)
  * -b | --branch \<name\> : 默认HEAD指向的分支名
  * -o \<name\> : 指定远程仓库别名默认为origin
  * --depth <数字n> : 只克隆最近的n次提交(暗含--single-branch)
  * --[no]single-branch : 仅克隆-b指定或默认远程HEAD指定的分支
  * --recurse-submodules[=<路径规范>] : 克隆子模块

### basic 快照

* git add

  * \<path\> : 文件或目录默认为当前目录
  * -i : 交互式
  * -u | --update : 只更新跟踪的文件的修改
  * -A | --all 添加所有更改(所有路径包括递归路径)
  * -v | --verbose : 显示详细信息(行)
  * -n : 显示如果add会添加哪些文件，但不实际执行
  * -f : 添加被.gitignore忽视的文件
* git status (工作数中的文件状态)

  * 默认输出：1.所在分支2.远程跟踪情况3.提交状态(工作树和暂存区)
  * -s | --short 简短输出
  * -b|--branch 即使在-s的时候也输出 `## branchname tracking info`
  * -v | --verbose 如同diff --cached;如果使用两次-v,如同git diff(也显示工作区的修改)
  * -u[mode[no|normal|all]]: 是否显示未跟踪文件，nomal显示未跟踪文件和文件夹,默认使all(normal+文件夹中的每一个文件)
  * --ignored[=[traditional（默认）|no|matching]] : 显示被忽略的文件
  * \<path\> : 指定路径或文件

* git diff(显示各种差异信息)

  * -- \<path\> : 
    * `git diff -- <path>` (默认)工作树和暂存区的差异
    * `git diff --staged|cached -- <path>` 暂存区和commit(默认HEAD)之间的差异
    * `git diff <commit> -- <path>` 工作树和指定commit之间的差异
    * `git diff <commitA> <commitB> -- <path>` 两次提交之间的差异B是最新的提交
  * --cached [--merge-base] \<commit\> -- \<path\> : 查看你为下一次提交所做的相对于\<commit\>(默认HEAD)的修改。 如果HEAD不存在且没有给出\<commit\>，显示所有已缓存的修改。 --staged是—cached的同义词。
  * --no-index -- \<path\> \<path\> : 文件系统上两个文件差异
  * [--merge-base] \<commit A\> \<commit\B> [--] [\<path\>] : 给出了 --merge-base，则使用两个提交的合并基础作为 "之前 "的一方。 git diff --merge-base A B `等同于`git diff $(git merge-base A B) B。如果如同以上只有一个<\commit\>,B 默认为HEAD

### branch

* git branch 
  * -d 删除分支
  * -D | --delete-forece 强制删除分支
  * -f | --force 将 <分支名> 重置为 <起始点>，即使 <分支名> 已经存在。
    * `git branch [--track[=(direct|inherit)] | --no-track] [-f] [--recurse-submodules] <分支名> [<起始点>]`
  * -m | --move 移动/重命名分支和reflog
    * -M ：-m -f的快捷
  * -c | --copy 复制分支
    * -C ：-c -f的快捷
  * -r | remotes 列出(删除与-d一起使用)所有远程分支
  * -a | --all 列出所有分支
  * -l | --list <pattern> 列出符合pattern的分支
  * -v -vv | --verbose 显示每个头的sha1和提交主题行，以及与上游分支的关系（如果有的话）。如果给了两次，也会打印链接的工作树的路径（如果有的话）和上游分支的名称
  * -t | --track[=direct|inherit] 在创建一个新分支时，设置 branch.<name>.remote 和 branch.<name>.merge 配置项来为新分支设置“上游”追踪。???不理解
  * --no-track 不要设置 “上游仓库” 配置，即使 branch.autoSetupMerge 配置变量为 true。???不理解
  * -u | --set-upstream-to=<upstream> 使<upstream>被视为<branchname>的上游分支。如果没有指定<branchname>，则默认为当前分支。
  * --unset-upstream 删除<branchname>的上游信息。如果没有指定分支，则默认为当前分支。
  * 筛选：--contains <commit>,--no-contains <commit>,--merged <commit>,--no-merged <commit>.
* git merge 将指定的提交并入当前分支HEAD
  * \<commit\> : 被合并的提交
  * --[no-]commit : 合并,是否提交，--commit可以覆盖no，配合--no-ff保证分支不更新
  * --no-ff | --ff | --ff-only : merge三种模式
  * --squash : 修改工作树和索引区如同merge成功一样，下一次git commit创建一个合并提交
  * -s <策略> -X [ours|theirs]: 合并策略和特定选项
  * -m \<message\> : 提交信息
  * -v : 详细信息
  * --abort : 终止当前的冲突解决过程。并尝试恢复合并前的状态(合并前不能有未提交的工作区变化)
  * --continue : 合并因为冲突终止后继续合并。
  * --quit : 忘记正在进行的合并。让工作区和索引保持原样(不常用,如果使用后续使用git commit提交)。
* git stash
  * 这在以下场景特别有用：需要切换分支，但代码还没写完;突然要修复一个紧急 bug;想拉取远程更新，但本地有未提交的修改
  * push
    * -m : stash信息
    * -u | --include-untracked | --no-include-untracked | --only-untracked : 是否包含未跟踪文件
    * -a | --all : 所有文件，包括忽略的(.log,.env带)
    * -p | --patch : 交互式选择部分修改保存
    * -k | --[no-]keep-index | : 保存工作区，暂存区不变
  * list,show(查看某个stash改了什么类似diff),drop,pop,apply; 不常用 clear,create,store
  * --index : 只有apply pop有效，尝试恢复索引区的变化，冲突时失败
  * --staged : 类似git commit,但是不是提交而是stash

### inspection

* git show: `git show [<选项>] [<对象>…]` 显示一个或多个对象（Blob、tree、tag和commit）。

  * <对象> : 默认为 HEAD
  * --pretty=\<格式\> | --format=\<格式\> : 格式可以是oneline,short,medium,full,fuller,reference,email,raw,format:\<string\>和tformat:\<string\>之一，默认为medium。
  * --name-only : 只显示文件名不显示diff差异
  * --name-status : 文件状态
  * --oneline : 一行
  * --no-patch | -s ; --patch -p : 是否显示差异
  * --stat : 显示每个文件的增删行数统计
  * --numstat : 以数字形式显示每个文件的增删行数
  * --word-diff : 按单词而不是行显示差异
  * --color-words : 着色的单词差异
  * --abbrev-commit : 短哈希
  * --follow \<path\> : 显示文件的记录包括重命名后
  * --parents : 显示父提交
  * --children : 显示子提交
* git log(如果是针对提交git log --no-walk -p等于git show,但是后者不止可以显式commit)

  * --follow \<path\> : 显示文件的记录包括重命名后
  * --no-decorate | --decorate=\<=short,full,auto,no\> : 显示引用的模式，默认是short.
  * --oneline : 一行
  * --pretty=\<format\> | --format=\<format\>: 同git show中的format
  * --name-only : 同git show
  * --stat : 同git show
  * --shortstat
  * --graph : 图形化
  * 范围数量控制
    * -n / -max-count
    * --since / --after
    * --until / --before
    * --author
    * --committer
    * --grep
    * \<since\>..\<until\> : 显示范围
    * --all : 所有分支
    * --branches : 所有或匹配分支commit
    * --tags : 所有或匹配的tags commit
  * diff 显示
    * -p | --patch | --no-patch | 是否显示差异
    * --word-diff : 单词差异
    * --color-words : 单词差异
    * --stat 文件变更统计
    * --patch-with-stat : 显示diff和stat
  * 合并与历史控制
    * --[no-]merges : 只或不显示合并提交
    * --first-parent : 只显示一个父提交
    * --no-walk : 我遍历父提交，只显示指定commit
* git shortlog : `git shortlog [<options>] [<revision-range>] [[--] <path>...]`,`git log --pretty=short | git shortlog [<options>]`两种方式等价，git shortlog 会读取 git log --pretty=short 格式的输入，然后进行分组统计。

  * git shortlog 的主要作用是：1.按作者分组提交2.排序作者（默认按作者名）3.统计每个作者的提交数量4.列出每个作者的提交标题（第一行消息）
  * -n 按提交数量降序
  * -s 不显示提交标题
  * -e 显示邮箱而不是作者名字
  * --committer 按committer分组而不是author分组
  * 指定范围与路径同git log
  * -c / --no-merges / -w / --format=\<format\> : 编码，排除合并提交，忽略空白差异，提交格式

## sharing

* git fetch:`git fetch [<options>] [<repository> [<refspec>...]]`

  * \<repository\> : 例如 origin,upstream远程仓库名
  * \<refsepc\>格式[+]\<src\>:\<dst\> : 分支或标签(main,dev等) 获取后，远程分支会以 origin/xxx 形式存储在本地（如 origin/main），你可以用 git log origin/main 查看。
    * \+ 强制更新(非快进)
    * src: 远程分支(如main)
    * dst: 本地存储位置(如refs/remotes/origin/main)
  * 选项
    * -v | --verbose 详细
    * -a | --all 所有远程仓库
    * -p | -prune 删除本地已失效的远程跟踪分支（即远程已删除的分支）
    * --[[no-]tags 获取远程的所有标签（tag）。默认 git fetch 不获取标签。
    * --depth=\<n\> 只获取最近 n 次提交
    * -f | --force 强制更新远程跟踪分支，即使不是快进（fast-forward）。慎用。通常用于修复被 push --force 覆盖的远程分支。
    * --dry-run 模拟获取过程，不实际下载任何数据
  * 引用规范具体举例
    * git fetch origin main 等价 git fetch origin main:refs/remotes/origin/main 作用：获取远程 main 分支，存为本地远程跟踪分支 origin/main。
    * git fetch origin main:local-main 作用：获取远程 main 分支创建或更新本地分支 local-main不会切换到该分支
    * git fetch origin +main:refs/remotes/origin/main + 表示强制更新即使远程 main 被 push --force 覆盖，本地也会强制同步.慎用：可能丢失本地对远程跟踪分支的修改（虽然一般不直接修改 origin/main）。
    * git fetch origin tag v1.0.0 等价 refs/tags/v1.0.0:refs/tags/v1.0.0
    * git fetch origin refs/pull/123/head:pr-123 GitHub PR 拉取
    * 通配符：refs/heads/*:refs/remotes/origin/* 获取远程所有分支映射为 origin/xxx 形式的远程跟踪分支+ 表示允许强制更新（如远程分支被 force push）默认规则
    * 查看当前 refspec：git config --get-all remote.origin.fetch
    * 修改 refspec（谨慎）：git config remote.origin.fetch "refs/heads/main:refs/remotes/origin/main"
    * \<dst\> 可以是任意引用：分支：refs/heads/temp远程跟踪：refs/remotes/origin/dev标签：refs/tags/temp自定义命名空间：refs/work/main-copy
* git pull(全局fetch,单独FETCH_HEAD merge)

  * `git pull [<选项>] [<仓库> [<引用规范>…]]` 的选项分为获取和合并个操作的选项
  * 获取的选项同git fetch
  * 合并的选项同git merge
  * 额外的选项
    * --autostash 自动暂存
    * --rebase[=false|true|preserve|interactive] 使用rebase而不是merge
    * --on-rebase
* git push

  * `git push [<选项>] [<仓库> [<引用规范>…]]`
  * \<引用规范\>=\<src\>:\<dst\>
  * \<src\> 通常是你想推送的分支的名字，但它可以是任何任意的“SHA-1 表达式”，比如 master~4 或 HEAD
  * \<dst\> 远程仓库的哪个引用被这个推送更新。这里不能使用任意的表达式，必须命名一个实际的引用。通常以 refs/ 开头，如：refs/heads/main refs/tags/v1.0.0
  * \<dst\> 的路径推断机制（当未以 refs/ 开头时）如果 \<dst\> 不是以 refs/ 开头（如 main 而不是 refs/heads/main），Git 会尝试自动推断其完整路径。\<src\> 是分支（refs/heads/*）-> \<dst\> → refs/heads/\<dst\> ; \<src\> 是标签（refs/tags/*）-> \<dst\> → refs/tags/\<dst\>

    * 什么是 git中的 refs
      * 在 Git 中，refs（读作 "references"）是一个指向提交对象（commit）的指针。文件路径：refs/heads/main 文件内容：一个 40 位的 SHA-1 哈希（如 a1b2c3d...），指向某个提交。
      * 常见的 refs 命名空间 Git 使用不同的前缀来分类管理不同类型的引用
        > refs/heads/ 本地分支
        > refs/tags/ 标签
        > refs/remotes/ 远程跟踪分支
        > refs/stash/ 暂存区
        > refs/notes/ 提交备注
        > refs/pull/ pull request临时引用(github风格)
        >
  * 例子：`git push origin main` 等价于 `git push origin main:main`git 推断为 `git push origin refs/heads/main:refs/heads/main`
  * 命令选项

    * --all | --branches 所有分支(即refs/heads下的引用)
    * --prune 删除没有本地对应分支的远程分支。
    * --mirror refs/下的所有引用都被镜像到远程仓库。
    * -- | --dry-run 做除了实际发送更新外的所有事
    * -d | --delete 所有列出的引用都从远程仓库中删除。这与在所有引用前加冒号的做法相同。
    * --tags 除了命令行上明确列出的引用规范之外，refs/tags 下的所有引用都被推送。
    * -u | --set-upstream 对于每一个已经更新或成功推送的分支，添加上游（跟踪）引用，由无参数的 git-pull和其他命令使用。
    * -v | --verbose 详细信息
    * --no-recurse-submodules | --recurse-submodules=check|on-demand|only|no 子模块推送的控制策略。
  * 最佳实践

    | 场景         | 命令                                     |
    | :----------- | :--------------------------------------- |
    | 正常推送     | `git push`（已设置 upstream）          |
    | 首次推送     | `git push -u origin <branch>`          |
    | 强制推送     | `git push --force-with-lease`          |
    | 推送标签     | `git push --tags` 或 `--follow-tags` |
    | 删除远程分支 | `git push -d origin <branch>`          |
    | CI/CD 集成   | `git push -o <key>=<value>`            |
    | 安全检查     | `git push -n` 预览                     |
* git remote

  | 命令                                                                       | 解释                       |
  | :------------------------------------------------------------------------- | :------------------------- |
  | git remote [-v\| --verbose]                                                | 列出所有已配置的远程仓库   |
  | git remote add [options]\<name\> \<URL\>                                   | 添加一个新的远程仓库       |
  | git remote rename\<old\> \<new\>                                           | 重命名远程仓库             |
  | git remote remove\<name\>                                                  | 删除远程仓库               |
  | git remote set-head\<name\> (-a \| --auto \| -d \| --delete \| \<branch\>) | 设置或删除远程的默认分支   |
  | git remote set-branches [--add]\<name\> \<branch\>...                      | 设置远程仓库要跟踪的分支   |
  | git remote get-url [--push] [--all]\<name\>                                | 获取远程仓库的 URL         |
  | git remote set-url [--push]\<name\> \<newurl\> [\<oldurl\>]                | 修改远程仓库的 URL         |
  | git remote set-url --add [--push]\<name\> \<newurl\>                       | 为远程仓库添加额外的 URL   |
  | git remote set-url --delete [--push]\<name\> \<URL\>                       | 删除一个额外的 URL         |
  | git remote show [-n]\<name\>...                                            | 显示远程仓库的详细信息     |
  | git remote prune [-n\| --dry-run] \<name\>...                              | 删除过期的远程跟踪分支     |
  | git remote update [-p\| --prune] [(\<group\> \| \<remote\>)...]            | 批量获取所有远程仓库的更新 |
* git branch

  * 查看分支
    * -r 远程分支
    * -a 所有分支
    * 选项
      * -v | --verbose 显示分支最新提交信息
      * -vv 更详细，上下游分支和提交差异(如[origin/main:ahead 2])
  * 筛选和排序
    * --[no-]merged \<commit\> ; --containes \<commit\> ; --points-at \<object\> ; --sort=\<key\> ; \<pattern\>
  * 创建分支 `git branch [options] <branch name> [start-point]`
    * -f 强制
    * --[no-]track=[direct 默认| inherit] 设置上游跟踪分支
    * --recurse-submodules 同时初始化子模块
  * 设置和取消上游 `git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branch>]` ; `git branch --unset-upstream [<branch-name>]`
  * 重命名分支 `git branch (-m|-M 强制) [<old>] <new>`
  * 复制分支 `git branch (-c|-C) [<old>] <new>`
  * 删除分支 `删除分支：git branch (-d|-D) [-r] <branch>...`
  * 编辑分支描述：`git branch --edit-description [<branch-name>]`
* git submodule 初始化、更新或检查子模块

  * 子命令
    * add,status,init,deinit,update,set-branch,set-url,summary,foreach,sync,absorbgitdirs.
* 查看远程分支和本地分支关联信息的命令：todo:b站视频git原理的git pull和push两段视频

### patching

* git rebase
  * 模式 1：`git rebase [<upstream>] [<branch>]` 将 \<branch\> 分支变基到 \<upstream\> 上,如果省略 \<branch\>，默认为当前分支。
  * 模式 2：`git rebase --onto <newbase> [<oldbase>] [<branch>]` 精确控制变基的起点和终点
  * 模式 3：`git rebase --root [<branch>]` 将整个分支从“根”开始变基将当前分支（或 \<branch\>）的所有提交，从第一个提交开始，变基到 main 之后,常用于完全重构一个长期分支的历史⚠️ 非常危险，会重写整个分支历史。
  * 模式 4：git rebase -i | --interactive 交互式变基——最强大的历史重写工具
  * 模式 5：git rebase --exec \<cmd\> 在每个提交后执行命令（常与 -i 结合）
  * 模式 6：git rebase --keep-base 保留原始基础分支的结构当你有一个长期分支，且上游（如 main）频繁更新时它会基于你最初分叉时的 main 版本进行变基，而不是最新的 main,避免重复解决冲突
  * 变基过程控制命令：--continue,--skip,--abort,--quit,--edit-todo,--show-current-patch
* git revert
  * 控制命令 `git revert (--continue | --skip | --abort | --quit)`
  * 模式1：`git revert [--[no-]edit] [-n] [-m <父提交数量>] [-s] [-S[<键 ID>]] <提交>…`
    * -e | --edit 在提交之前编辑提交信息
    * -m [父编号] 通常情况下，由于不知道合并的哪一方应被视为主线，因此无法还原合并。 该选项指定了主线的父线编号（从 1 开始），允许还原指令相对于指定的父线反向更改。
    * -n | --on-commit 修改工作区和索引区但是不提交
  * 例子：
    * git revert HEAD^3 还原 HEAD 中倒数第四个提交指定的更改，并创建一个包含还原更改的新提交。
    * git revert -n master~5..master~2 将 master 中倒数第五次提交（包含）到 master 中倒数第三次提交（包含）的改动还原，但不创建任何包含还原改动的提交。还原只会修改工作区和索引。
* git cherry-pick 给出一个或多个现有的提交，应用每个提交所带来的变化，为每个提交记录一个新的提交。 这需要您的工作区是干净的（没有对 HEAD 提交的修改）。
  * 使用场景解释,假设你有两个分支，一个v1.0,一个v2.0,v2.0中有很多新的特性或者修改的commits是领先的,可能v2.0中进行了一个commit,这个提交可能是bugfix,或者是一个通用的特性，需要同时加入到v1.0和v2.0中，但是你不能把v2.0合并入v1.0,因为很多commit是v1.0不需要的，你不得不手动修改v1.0,但使用cherry-pick你就可以选择部分提交作为另一个分支的提交。
  * 模式1：`git cherry-pick [--edit] [-n] [-m parent-number] [-s] [-x] [--ff] [-S[<键 ID>]] <提交>…`
    * -e | --edit 提交前编辑提交信息
    * -x 在记录提交时，在原始提交信息中添加一行 "(cherry picked from commit …)"，以表明这个改动是从哪个提交中拣选的。只能用于公共分支之间的 cherry-pick
    * -m | --mainline \<父提交数量\> 选择合并的父编号从1开始
    * -n | --on-commit 修改工作区和索引区，而不提交
    * -s / -S 提交信息尾注 / GPG签名提交
    * --ff 如果当前的 HEAD 与拣选的提交的父本相同，那么将执行快速合并到这个提交。
  * 控制命令
    * --continue 继续执行
    * --skip 跳过当前提交，继续下一个（如果有多个提交要 pick）。
    * --quit 退出 cherry-pick 过程，但保留工作区和暂存区的更改。
    * --abort 取消操作工作区和暂存区恢复原状。
  * 用途：将某个修复、功能提交从一个分支（如 hotfix）应用到另一个分支（如 main 或 develop），而无需合并整个分支。
