修改ssh_config文件

vim /etc/ssh/ssh_config 

添加参数

ClientAliveInterval 600  //600秒
ClientAliveCountMax 5 //延期5次断开

重启sshd服务

systemctl restart sshd