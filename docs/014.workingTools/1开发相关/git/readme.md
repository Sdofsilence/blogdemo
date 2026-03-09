# learn git 资源

* learn git branching <https://learngitbranching.js.org/?locale=zh_CN>
* 用户手册 <https://git-scm.cn/docs/user-manual>
* git - the simple guide <https://rogerdudler.github.io/git-guide/>
* git中文参考 <https://git.apachecn.org/#/> ApacheCN 由 iBooker布客团队 建立的公益性文档和教程翻译项目
* git教程廖雪峰<https://liaoxuefeng.com/books/git/introduction/index.html> 适合快速查看回顾
* GitHub Training Kit<https://training.github.com/> 
* Github git-cheat-sheet<https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/> 常用命令
* git对svn的支持：<https://training.github.com/downloads/subversion-migration/>

## 官方文档文章

* 7.7章节 Git工具-重置揭密<https://git-scm.com/book/zh/v2/Git-%e5%b7%a5%e5%85%b7-%e9%87%8d%e7%bd%ae%e6%8f%ad%e5%af%86> 关于 reset 和 checkout 是否对HEAD index WorkDir作出修改

| * | HEAD | index | WorkDir | WorkDir Safe |
|:---|:--|:--|:--|:--|
| **Commit Level**|  |  |  |  |
| reset --soft [commit] | REF | NO | NO | YES |
| reset [--mixed] [commit] | REF | YES | NO | YES |
| reset --hard [commit] | REF | YES | YES | NO |
| checkout \<commit\> | HEAD | YES | YES | YES |
| **File Level** |  |  |  |  |
| reset [commit] \<paths\> | NO | YES | NO | YES |
| checkout [commit] \<paths\> | NO | YES | YES | NO |

上面表格的意思就是reset针对commit的时候，会修改HEAD指针指向的分支的ref到commit;checkout 则是仅仅修改HEAD自身，使得当前的workdir,index和HEAD三者都匹配某个commit

而指定文件的时候，不会修改HEAD或指向的分支ref,而是仅仅影响index或者workdir。

* 第十章 git 内部原理<https://git-scm.com/book/zh/v2/Git-%e5%86%85%e9%83%a8%e5%8e%9f%e7%90%86-%e5%ba%95%e5%b1%82%e5%91%bd%e4%bb%a4%e4%b8%8e%e4%b8%8a%e5%b1%82%e5%91%bd%e4%bb%a4>

## 博客或文章

* 51cto 图解24个核心Git命令<https://www.51cto.com/article/822388.html>
* 稀土掘金 Git入门图文教程(1.5W字40图)<https://juejin.cn/post/7195030726096453690>
* github gitignore各种语言项目的配置文件<https://github.com/github/gitignore>
* 8 个让我更有效率的 Git 别名<https://cn.linux-console.net/?p=33283>
* 七个改变我生活的 Git 小技巧<https://zhuanlan.zhihu.com/p/384645874>
* 如何撤消 Git 中的更改（重置、恢复、恢复）<https://blog.git-init.com/how-to-undo-changes-in-git-using-reset-revert-and-restore/>
* 7.7 Git 工具 - 重置揭密<https://git-scm.com/book/zh/v2/Git-%e5%b7%a5%e5%85%b7-%e9%87%8d%e7%bd%ae%e6%8f%ad%e5%af%86>
* git docs reference常用命令分类解析<https://git-scm.com/docs>
