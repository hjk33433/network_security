开机按e进入编辑模式

将ro 改为rw  行尾添加init=/bin/bash 进入单用户模式
passwd root

mkfifo /run/initctl        #保存操作
reboot -f           #强制重启
