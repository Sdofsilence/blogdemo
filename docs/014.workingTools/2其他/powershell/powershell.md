# powershell 学习记录

* 常见powershell命令简写大全<https://blog.csdn.net/2301_79518550/article/details/145692315>

* 知乎 powershell自动补全<https://zhuanlan.zhihu.com/p/634110021>

* csdn powershell自动补全<https://blog.csdn.net/m0_63230155/article/details/134685660>

* microsoft learn 在shell中使用Tab键补全<https://learn.microsoft.com/zh-cn/powershell/scripting/learn/shell/tab-completion?view=powershell-7.5>

* PSReadLine module<https://learn.microsoft.com/en-us/powershell/module/psreadline/?view=powershell-7.5>

* 配置 PowerShell PSReadLine 模块<https://www.fournoas.com/posts/configuring-psreadline-module-of-powershell/>

* carapace-bin(PSReadLine的补充)<https://github.com/carapace-sh/carapace-bin> <https://carapace-sh.github.io/carapace-bin/carapace-bin.html>

## 语法

* **《reference》** <https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.5> **中的about_可以很简单快速的理解相关语法或者概念，主要去看这个链接，其他的太啰唆了**
* powershell 语言规范3.0<https://learn.microsoft.com/zh-cn/powershell/scripting/lang-spec/chapter-01?view=powershell-7.5>
* micorsoft learn powershell 101:<https://learn.microsoft.com/zh-cn/powershell/scripting/learn/ps101/00-introduction?view=powershell-7.5>
* 运算符<https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.5>

### 101
其中提到的命令很常用，概念有点分散，但是都是重要的(其他参考reference,)
#### 第一章 - 开始使用
* 执行策略 PowerShell 中的执行策略是一项安全功能，旨在帮助防止无意中执行恶意脚本。 但是，这不是安全边界，因为它无法阻止确定的用户故意运行脚本。 确定的用户可以绕过 PowerShell 中的执行策略。
  * 可以为本地计算机、当前用户或 PowerShell 会话设置执行策略。 还可以为具有组策略的用户和计算机设置执行策略。
  * 无论执行策略设置如何，都可以以交互方式运行任何 PowerShell 命令。 执行策略仅影响脚本中运行的命令。 使用 Get-ExecutionPolicy cmdlet 确定当前的执行策略设置。
  * 若要启用脚本的执行，请使用 cmdlet 更改执行策略 Set-ExecutionPolicy 。 如果未指定 Scope 参数， 则为默认范围为 LocalMachine 。 必须以管理员身份运行 PowerShell 才能更改本地计算机的执行策略。 除非对脚本进行签名，否则建议使用 RemoteSigned 执行策略。 RemoteSigned 阻止运行未由受信任的发布者签名的已下载脚本。
#### 第二章 - 帮助系统 
* powershell 中的三个核心命令：Get-Help,Get-Command,Get-Member
    * `help -name command [-Full -Detail -Examples -Online -ShowWindow  [-ParameterName parameterName]]
    * `get-command [-Name,-Noun,-Verb(都支持通配符)] command -Syntax -CommandType`
#### 第三章 - 发现对象、属性和方法
* Get-Member
  * `[例子：get-service] | get-member`
  * `[例子：get-service] | select -Property name,status,can*`
* Method
  * `[例子：Get-Service] | Get-Member -MemberType Method`
  * `(Get-Service -Name w32time).Stop()` 可以使用stop方法停止服务
  * `Get-Service -Name w32time | Start-Service -PassThru`参数-PassThru让没有输出的cmdlet输出对象(有输出)
  * `Start-Service -Name w32time | Get-Member`报错 但是`Start-Service -Name w32time -PassThru | Get-Member`成功因为指定了-PassThru参数
* Get-Command
  * `Get-Command -ParameterType ServiceController` 通过命令对象生成的类型筛选
#### 第四章 - 准确描述和管道
* 单行式命令
  * 自然换行符可以出现在常用的字符中，包括管道符'|',逗号,左括号'['、大括号'{' 和括号'('。 其他不太常见的符号包括分号';'、等号'='，单引号和双引号的起始部分（'，"）。
  * > Get-Service | \
    > Where-Object CanPauseAndContinue -EQ $true | \
    > Select-Object -Property *
  * shift+enter也可以在控制台强制换行
  * 下一个示例不符合 PowerShell 单行式命令的条件，因为它不是一个连续的管道。 相反，它是放在一行上的两个单独的命令，用分号分隔。 此分号表示一个命令的末尾和另一个命令的开头。
  * `$Service = 'w32time'; Get-Service -Name $Service`
  * 许多编程和脚本语言都需要每行末尾的分号。 但是，在 PowerShell 中，行尾的分号是不必要的，不建议这样做。 应该避免使用它们，以使代码更简洁且更易于阅读。
* 左侧筛选器(本章演示如何筛选各种命令的结果)
  * 使用`Get-Service -Name w32time`而不是`Get-Service | Where-Object Name -EQ w32time`
* 用于有效筛选的命令排序
  * 有一个误解，即 PowerShell 中的命令顺序是无关紧要的，但这是一个误解。 排列命令的顺序（尤其是在筛选时）非常重要。
  * > 不使用,sleect-object -Property导致属性被排除\
    > Get-Service |\
    > Select-Object -Property DisplayName, Running, \Status |\
    > Where-Object CanPauseAndContinue
  * > 使用\
    > Get-Service |\
    > Where-Object CanPauseAndContinue |\
    > Select-Object -Property DisplayName, Status
* 管道 todo:说的太复杂了，没看懂，但是很重要，阐述了管道之间的两个命令的对象传递和赋值方式(前一个命令的结果如何被后一个命令的参数绑定)<https://learn.microsoft.com/zh-cn/powershell/scripting/learn/ps101/04-pipelines?view=powershell-7.5#the-pipeline>
  * 大概的意思就是可以通过get-help/help/man获取命令的inputs/outputs信息，可以知道命令的输入输出对象是什么，重点是输入参数，一个命令的输入参数可以通过管道传输的前提是命令的参数中有参数属性AcceptPipelineInput为true的参数。
  * 在处理管道输入时，如果一个参数同时接受按属性名称管道输入和按值管道输入，则优先考虑按值绑定。 如果此方法失败，则尝试按属性名称处理管道输入 。 然而，术语按值可能具有误导性。 更准确的描述是按类型。例如，如果将生成 ServiceController 对象的命令的输出传递给 Stop-Service，则此输出将绑定到 InputObject 参数。 如果管道命令生成一个 String 对象，则会将输出与 Name 参数相关联。 如果从一个不产生 ServiceController 或String 对象但包含名为 Name 的属性的命令通过管道传输，则 Stop-Service 会将 Name 属性的值绑定到其 Name 参数。(简单说就是byvalue是匹配类型，bypropertyName是当对象类型不匹配时候匹配对象的Name属性赋值给Name参数(这里Name参数是个例子,其他参数同理))
  *  如何解决不匹配？使用 Select-Object 重命名属性，使其与目标参数名匹配：`Select-Object -Property *,@{Name='Name'; Expression={$_.ServiceName}}`多个属性使用多个@{},;或者修改对象的属性成员add-member,remove-member,set-member...;
* PowerShellGet 模块:其实就是find-module相关的命令`gcm -Module powershellget`

#### 第 5 章 - 格式、别名、提供程序、比较
* 从右往左格式化数据
  * 我们在第 4 章中了解了尽可能向左进行筛选。 手动格式化命令输出的规则与该规则类似，但需要尽可能向右进行筛选。
  * 见的格式命令包括 Format-Table 和 Format-List。 也可以使用 Format-Wide 和 Format-Custom；除非使用自定义格式，否则返回四个以上属性的命令将默认为列表。
  * 由于这些格式命令会导致输出的类型改变。这意味着，不能通过管道将格式命令传送到大多数其他命令。 它们可以通过管道传递给某些 Out-* 命令，但就是这样。 因此，建议你在行尾执行任何格式化操作（从右往左格式化数据）。
* 别名
  * 查找命令的别名，则需要使用 Definition 参数。`Get-Alias -Definition Get-Command, Get-Member`
* Provider
  * PowerShell 中有多个内置提供程序。`Get-PSProvider`
  * 使用 Get-PSDrive cmdlet 时，可以确定这些提供程序用于公开其数据存储的实际驱动器。 Get-PSDrive cmdlet 不仅显示由提供程序公开的驱动器，还显示 Windows 逻辑驱动器，包括映射到网络共享的驱动器。`Get-PSDrive`
  * 第三方模块（如 ActiveDirectory PowerShell 模块和 SqlServer PowerShell 模块）都会添加自己的 PowerShell 提供程序和 PSDrive。
  * 可以像传统文件系统那样访问 PSDrive。`Get-ChildItem -Path Cert:\LocalMachine\CA`
* 比较运算符
  * 通常将比较运算符与 Where-Object cmdlet 结合使用来执行筛选。

  |operator|
  |:---|
  |-eq|
  |-ne|
  |-gt|
  |-ge|
  |-lt|
  |-le|
  |-like|
  |-notlike|
  |-match|
  |-notmatch|
  |-contains|
  |-notcontains|
  |-in|
  |-notin|
  |-replace|

  * 带c前缀的比较运算符区分大小写例如`-ceq`
  * 容易混淆的 -like 和 -match 运算符。 
    * -like 与通配符 * 和 ? 搭配使用，以执行“like”匹配。
    * -match 运算符使用正则表达式来执行匹配。
    * contains 和 in 运算符，分别处理两种逻辑：
      * $array -contains $value
      * $value -in $array
  * -replace 运算符它用于替换某些内容。 指定一个值时，会将该值替换为无值。
    * `'PowerShell' -replace 'Shell'`得到`Power`
    * 如果要将值替换为其他值，请在要替换的模式后指定新值。
    * 还有一些方法（如 Replace() 可用于替换类似于 replace 运算符工作原理的内容。 但是，默认情况下 -replace 运算符不区分大小写，而 Replace() 方法区分大小写。
#### 循环
* ForEach-Object
* for
* do until
* do while
* while
* break,continue,return
#### 远程处理 见原文
#### functions 见原文
#### 模块 见原文