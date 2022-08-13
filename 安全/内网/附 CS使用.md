# 介绍
团队渗透工具
# 安装CS
1. 部署java
2. 启动teamserver ip 密码
# 建立beacon
1. 建立监听器
2. 设置payload
3. 目标机执行命令payload
4. 建立连接
# 模块讲解
## cs
- new connection 新建连接
- preferences 设置
- visualization 输出结果
- vpn interface vpn接口
- listeners 监听器
- script manager 脚本管理
- close 退出连接
## view
application 显示受害者主机
credentials 凭证
download 下载的文件
event log 主机上线记录 团队聊天
keystrokes 查看键盘记录
proxy pivote 查看代理
screenshots 查看截图
script console 加载第三方脚本
targets 显示所有受害者主机
web log 显示所有web服务日志
## attacks
- html application executable/vba/ps 的hta木马
- ms office macro offic 宏病毒木马
- payload generator 生成payload
- use/cd autoplay 自动播放木马文件
- windows dropper 对任意文件捆绑木马
- windows executable 可执行木马
- windows executable（stageless）生成无状态可执行木马

## Web Drive-by模块
Manage # 对开启的web服务进行管理
Clone Site # 克隆网站，可以记录受害者提交的数据
Host File # 提供文件下载，可以选择Mime类型
Scripted Web Delivery # 为payload提供web服务以便下载和执行，类似于
Metasploit的web_delivery
Signed Applet Attack # 使用java自签名的程序进行钓鱼攻击(该方法已过时)
Smart Applet Attack # 自动检测java版本并进行攻击，针对Java 1.6.0_45以下以
及Java 1.7.0_21以下版本(该方法已过时)
System Profiler # 用来获取系统信息，如系统版本，Flash版本，浏览器版本等
Spear Phish # 鱼叉钓鱼邮件


## reporting模块
Activity Report # 活动报告
Hosts Report # 主机报告
Indicators of Compromise # IOC报告：包括C2配置文件的流量分析、域名、IP和
上传文件的MD5 hashes
Sessions Report # 会话报告
Social Engineering Report # 社会工程报告：包括鱼叉钓鱼邮件及点击记录
Tactics, Techniques, and Procedures # 战术技术及相关程序报告：包括行动对应
的每种战术的检测策略和缓解策略
Reset Data # 重置数据
Export Data # 导出数据，导出.tsv文件格式

## help模块
Homepage # 官方主页
Support # 技术支持
Arsenal # 开发者
System information # 版本信息
About # 关于

![c90546b39a252c15d06933bf79fbe660.png](../../_resources/c90546b39a252c15d06933bf79fbe660.png)

## 工具栏
1.新建连接
2.断开当前连接
3.监听器
4.改变视图为Pivot Graph(视图列表)
5.改变视图为Session Table(会话列表)
6.改变视图为Target Table(目标列表)
7.显示所有以获取的受害主机的凭证
8.查看已下载文件
9.查看键盘记录结果
10.查看屏幕截图
11.生成无状态的可执行exe木马
12.使用java自签名的程序进行钓鱼攻击
13.生成office宏病毒文件
14.为payload提供web服务以便下载和执行
15.提供文件下载，可以选择Mime类型
16.管理Cobalt Strike上运行的web服务
17.帮助
18.关于