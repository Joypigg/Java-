查找指定用户名进程：pgrep java |xargs ps -u --pid

- 增加用户：`useradd -d /usr/username -m username`
- 为用户增加密码：`passwd username`
- 新建工作组：`groupadd groupname`
- 将用户添加进工作组：`usermod -G groupname username`
- 删除用户：`userdel username`

init   0为关机、1为重启

reboot主机重启

halt、shutdown关机

stat  filepath查看文件

wc -c filename  得到字节数

du  -b  filepath  字节数

du -h  filepath  文件大小

rm -rf   目录名字    删除目录

rmdir  目录        删除目录

rm  -f  *    删除文件

cat    查看文件

打开系统启动管理msconfig

注册表regedit

进入终端ctrl+alt+t