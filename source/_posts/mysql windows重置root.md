MySQL
Windows下忘记root密码重置方式：
1.关闭 运行中的MySQL服务
2.打开dos窗口，转到mysql/bin目录
3.输入 mysqld --skip-grant-tables 回车 （--skip-grant-tables的意思是启动MySQL服务时候跳过权限表认证）
4.再打开一个dos窗口 （因为刚才的dos窗口无法使用了）转到mysql/bin目录
5.输入mysql回车，如果成功，将会出现mysql>符号
6.连接权限数据库：use mysql；
7.改密码： update user set password=password("123") where user="root"；别忘记加分号
8.刷新权限（必须步骤）：flush privileges;
9.退出quit
10.注销系统，重新进入