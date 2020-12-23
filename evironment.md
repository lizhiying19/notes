### 查看cuda版本
```SHELL
nvcc --version
```

### uname命令
uname命令用于打印当前系统相关信息（内核版本号、硬件架构、主机名称和操作系统类型等
```SHELL
uname [选项]

-a或--all：显示全部的信息；
-m或--machine：显示电脑类型；
-n或-nodename：显示在网络上的主机名称；
-r或--release：显示操作系统的发行编号；
-s或--sysname：显示操作系统名称；
-v：显示操作系统的版本；
-p或--processor：输出处理器类型或"unknown"；
-i或--hardware-platform：输出硬件平台或"unknown"；
-o或--operating-system：输出操作系统名称；
--help：显示帮助；
--version：显示版本信息。
```

### lsb_release命令
LSB是Linux Standard Base的缩写，lsb_release命令用来显示LSB和特定版本的相关信息。如果使用该命令时不带参数，则默认加上-v参数。
```SHELL
-v 显示版本信息。
-i 显示发行版的id。
-d 显示该发行版的描述信息。
-r 显示当前系统是发行版的具体版本号。
-c 发行版代号。
-a 显示上面的所有信息。
-h 显示帮助信息。
``` 