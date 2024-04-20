# powershell 记录

## 1. 获取环境变量

> 参考：[微软官方文档](https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.4)

Windows 上环境变量有三个作用域(scopes)。
- 计算机（系统）作用域
- 用户作用域
- 流程范围（当前进程或 POWERSHELL 会话的环境变量）

通过 POWESHELL 修改环境变量只能修改**当前会话**的环境变量，除非使用 System.Environment 类方法

### 使用变量语法（Using the variable syntax）

1. show
```powershell
$Env:<variable-name>
```
2. new
```powershell
$Env:<variable-name> = "<new-value>"
```
3. update
```powershell
$Env:<variable-name> = "<new-value>"
```
4. delete
```powershell
$Env:<variable-name> = ""
```

### 使用环境变量提供项和项 cmdlt

1. show
```powershell
Get-Item -Path Env:\Foo
```
2. new
```powershell
New-Item -Path Env:\Foo -Value 'Bar'
```
3. update
```powershell
Set-Item -Path Env:\Foo -Value 'Tar'
```
4. delete
```powershell
Remove-Item -Path Env:\Foo -Verbose
```
5. copy
```powershell
Copy-Item -Path Env:\Foo -Destination Env:\Foo2 -PassThru
```
> 使用 Get-ChildItem cmdlet 查看环境变量的完整列表
> Get-ChildItem Env:

### 使用 System.Environment 方法

1. show
```powershell
[Environment]::GetEnvironmentVariable('Foo')
```
2. new
```powershell
[Environment]::SetEnvironmentVariable('Foo','Bar')
```
3. update
```powershell
[Environment]::SetEnvironmentVariable('Foo','Tar')
```
4. delete
```powershell
[Environment]::SetEnvironmentVariable('Foo','')
```

### TIPS
1. 添加内容：`$Env:Path += ';C:\Tools'`，在 Windows 中使用 `;` 而不是 `:`。
2. 获取 powershell 配置文件位置：`$PROFILE`
