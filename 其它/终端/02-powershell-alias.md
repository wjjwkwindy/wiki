# powershell alias 其它命令

## 1.使用 powershell 的 function 来实现

```bash
PS> function get-gitstatus { git status }

PS> get-gitstatus
# On branch master
nothing to commit (working directory clean)

PS> Set-Alias -Name gs -Value get-gitstatus

PS> gs
# On branch master
nothing to commit (working directory clean)
```

技术点：  
PS > function [name] { [command] }  
PS > Set-Alias -Name [name] -Value [command]

## 2.持久化保存

> 使用 profile 来保存一些初始化工作  
> 四种不同的 profile 脚本  
> $pshome 为 C:\Windows\System32\WindowsPowerShell\v1.0 可以将 example 文件夹中的 profile.ps1 复制出来

|    Profile    |      描述      |  位置 |
|:--------------|:-----------------------------------------|:------|
|所有用户        |所有用户共有的profile                      |$pshome\profile.ps1 |
|所有用户（私有） |powershell.exe 中验证。                   |$pshome\Microsoft.PowerShell_profile.ps1|
|当前用户        |当前用户的profile                         |$((Split-Path $profile -Parent)+ “\profile.ps1”)|
|当前用户（私有） |当前用户的profile；只在Powershell.exe中验证 |$profile |

```ps1
#  Copyright (c) Microsoft Corporation.  All rights reserved.
#  
# THIS SAMPLE CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
# WHETHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A PARTICULAR PURPOSE.
# IF THIS CODE AND INFORMATION IS MODIFIED, THE ENTIRE RISK OF USE OR RESULTS IN
# CONNECTION WITH THE USE OF THIS CODE AND INFORMATION REMAINS WITH THE USER.
function get-gitlog{ git log --oneline --decorate --color --graph }
set-alias -name glog -value get-gitlog
```
