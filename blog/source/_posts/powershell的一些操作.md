---
title: powershell的一些x操作
date: 2018-01-19 17:27:28
tags:
---
### powershell中使用vim编辑器
习惯使用linux的shell的用户在使用window时会习惯的使用powershell进行操作，而vim编辑器则是linux系统的必备编辑器了，如何在windows的powershell中使用vim命令调用vim编辑器呢？
### powershell的一些命令
linux中shell的命令大部分同样在powershll中可用.

<!---more--->
#### powershell中使用vim编辑器

---

1、下载安装vim编辑器：要使用vim命令在命令窗口打开vim编辑器首先得有vim编辑器软件，windows系统是默认的编辑器是记事本，所以得自己安装vim编辑器

[vim编辑器下载地址](https://en.softonic.com/download/vim/windows/post-download?sl=1)

2、安装，在windows上安装，直接下一步就可以了，在安装时可以改变安装的位置

3、以管理员的身份运行powershell，执行Set-ExecutionPolicy RemoteSigned命令，在对话框中选择Y，如下：


```
PS C:\Windows\system32> Set-ExecutionPolicy RemoteSigned
执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 // http://go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [N] 否(N)  [S] 挂起(S)  [?] 帮助 (默认值为“Y”): Y
PS C:\Windows\system32>
```
4、使用new-item命令，创建一个PowerShell的配置文件Profile。如图

```
PS C:\WINDOWS\system32> new-item -path $profile -itemtype file -force
```
5、编辑配置文件（notepad $profile：调用记事本打开配置文件），在打开的记事本中添加vim相关的alias和如下代码。
```
set-alias vim "C:/Program Files/Vim/Vim74/./vim.exe" //后面引号部分是你vim安装的地方

# To edit the Powershell Profile
# (Not that I'll remember this)
Function Edit-Profile
{
    vim $profile
}

# To edit Vim settings
Function Edit-Vimrc
{
    vim $HOME\_vimrc
}
```
6、添加上述代码后，保存关闭记事本，然后重启PowerShell后，就可以正常使用了，输入vim即可在powershell中打开vim编辑器了。

---

#### powershell的一些命令
