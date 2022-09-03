### Scoop-configuration 
### (Windows Package Management Tool)

#### Scoop安装(https://scoop.sh/) GitHub地址(https://github.com/ScoopInstaller/Scoop)
#### 一、准备 (运行 Windows PowerShell)
##### 1.解决网络问题（DNS默认阿里云）
###### ping raw.githubusercontent.com 若无法访问进入

    C:\Windows\System32\drivers\etc\hosts

###### 记事本(Notepad++)打开，文件末尾添加

    199.232.68.133 raw.githubusercontent.com

###### 保存刷新重新 ping ，成功关闭

    ipconfig /flushdns #刷新

##### 2.Windows问题(正常 Windows10 新版无需查看)
###### 查看 PowerShell
###### 更新 https://github.com/PowerShell/PowerShell 在`Downloads (preview)`内下载对应平台的预览版，预览版好用

    echo $psversiontable #查看powershell信息
    $PSVersionTable.PSVersion #新版跳过 version 5.1 or later

###### 查看 .NET Framework 版本
###### 更新 https://dotnet.microsoft.com/download/dotnet-framework

    $PSVersionTable.CLRVersion #新版跳过 version 4.5 or later (install recommended)

###### 策略配置

    Set-ExecutionPolicy RemoteSigned -scope CurrentUser
    Get-ExecutionPolicy -List #查看执行策略设置，检查是否设置成功

//><//
#### 二、安装
##### 切记：Scoop每次安装失败必须删除路径文件夹(默认为 C:\Users\<user>\scoop)
##### 1.输入命令，scoop默认安装，如需自定义路径不要使用该命令，默认（C:\Users\<user>\scoop）Global默认位置是（C:\ProgramData\scoop）

    iwr -useb get.scoop.sh | iex

##### 2.未能创建 SSL/TLS 安全通道,输入下面命令再安装，若是新系统，打开IE-->使用推荐设置窗口内，点击确定。（注意需要删除scoop安装包删除再装）

    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

##### 3.自定义安装（这里修改为D盘Scoop，可选，推荐使用）

    irm get.scoop.sh -outfile 'install.ps1'
    .\install.ps1 -?
    .\install.ps1 -ScoopDir 'D:\Scoop' -ScoopGlobalDir 'D:\Scoop\' -NoProxy  #点击回车安装

##### 4.修改scoop安装位置，此为旧方法不推荐使用,仅作记录）
##### 注意：旧自定义安装，需按序执行环境变量配置命令，否则scoop还是默认位置安装。

    //开始配置(这里修改为D盘Scoop，可选)
    $env:SCOOP='D:\Scoop'
    [Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
    # run the installer

    //配置全局程序安装路径(这里修改为F盘GlobalScoopApps，可选)
    $env:SCOOP_GLOBAL='F:\GlobalScoopApps'
    [Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine')
    # run the installer

    //路径配置完成执行此条安装结束
    irm get.scoop.sh | iex

    //若网络有问题，也可以使用代理安装，修改命令中的代理网址即可
    irm get.scoop.sh -Proxy 'http://<ip:port>' | iex  

//><//
#### 三、日常补充（Scoop was installed successfully!）
##### 1.Github加速 解决Scoop update更新慢的问题
###### 下载地址 https://github.com/dotnetcore/FastGithub
###### 解压进入文件夹 右击`终端打开`或者`powershell`打开输入

    ./fastgithub start

    git config --global http.sslverify false #解决Git的SSL证书问题

##### 2.Scoop使用 [scoop 操作 对象] 例如：scoop install vim
###### 安装aria2提高速度（C:\Users\<user>\.config\scoop）配置修改

    scoop install aria2 #多线程，默认开
    scoop config aria2-max-connection-per-server 16
    scoop config aria2-split 16
    scoop config aria2-min-split-size 1M

###### 添加库

    scoop bucket add extras #添加extras库
    scoop bucket add java   #添加Java库
    scoop update #添加库后必须更新

###### 卸载

    scoop uninstall scoop #卸载scoop

