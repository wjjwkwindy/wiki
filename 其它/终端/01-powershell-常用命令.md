# powershell 常用命令

## 查看 powershell 的版本

```bash
PS> $Host.Version
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      18362  145

PS> (Get-Host).Version
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      18362  145

PS> Get-Host
Name             : ConsoleHost
Version          : 5.1.18362.145
InstanceId       : af3c77dd-0f30-4e2e-97e5-fd583bb34a6b
UI               : System.Management.Automation.Internal.Host.InternalHostUserInterface
CurrentCulture   : zh-CN
CurrentUICulture : zh-CN
PrivateData      : Microsoft.PowerShell.ConsoleHost+ConsoleColorProxy
DebuggerEnabled  : True
IsRunspacePushed : False
Runspace         : System.Management.Automation.Runspaces.LocalRunspace
```

## `Get-Command` | `gcm` 查看命令

```bash
PS> Get-Command Set-Alias
CommandType     Name              Version    Source
-----------     ----              -------    ------
Cmdlet          Set-Alias         3.1.0.0    Microsoft.PowerShell.Utility

PS> gcm Set-Alias
CommandType     Name              Version    Source
-----------     ----              -------    ------
Cmdlet          Set-Alias         3.1.0.0    Microsoft.PowerShell.Utility
```

## `Set-Alias` 设置别名

```bash
PS> Set-Alias -Name dir -Value Get-ChildItem

PS> Get-Alias -Name dir

CommandType     Name
-----------     ----
Alias           dir -> Get-ChildItem
```

## `Remove-Alias` | `Remove-Item` 删除别名

```bash
PS> Remove-Alias -Name dir

PS> Remove-Item Alias:dir (低于5.x版本)
```

## `Invoke-Item` | `ii` 从 powershell 中打开资源管理器

```bash
PS> Invoke-Item .
or
PS> ii .
```
