# powershell alias 其它命令

> 使用 powershell 的 function 来实现

```shell
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
