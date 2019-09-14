bypass_disablefunc.php有三个GET选项：cmd，outpath和sopath。

选项非常简单，

cmd是你的邪恶命令，

outpath是命令输出文件路径（可读和可写），

sopath是我们的共享对象bypass_disablefunc_x64.so的绝对路径。例如：

http://site.com/bypass_disablefunc.php?cmd=pwd&outpath=/tmp/xx&sopath=/var/www/bypass_disablefunc_x64.so


一是cmd参数，待执行的系统命令（如pwd）;

二是outpath参数，保存命令执行输出结果的文件路径（如/ tmp / xx），便于在页面上显示，另外该参数，你应注意web是否有读写权限，web是否可跨目录访问，文件将被覆盖和删除等几点;

三是sopath参数，指定劫持系统函数的共享对象的绝对路径（如/var/www/bypass_disablefunc_x64.so），另外关于该参数，你应注意web是否可跨目录访问到它。

此外，bypass_disablefunc.php拼接命令和输出路径成为完整的命令行，所以你不用在cmd参数中重定向。


bypass_disablefunc_x64.so为执行命令的共享对象，用命令gcc -shared -fPIC bypass_disablefunc.c -o bypass_disablefunc_x64.so

将bypass_disablefunc.c编译而来。若目标为x86架构，需要加上-m32选项重新编译bypass_disablefunc_x86.so


想办法将bypass_disablefunc.php和bypass_disablefunc_x64.so传到目标，指定好三个GET参数后，bypass_disablefunc.php即可突破disable_functions





已知问题：

1）不同目标环境可能需要重新编译共享对象。本项目中的bypass_disablefunc_x64.so是在debian，x64下编译的，在非debian系可能需要重新编译;
2）bypass_disablefunc.php需要对outpath指定的路径具有读写权限，若目标开启SELinux可能阻止web账号写文件。如，你用apache身份运行id，输出uid=48(apache) gid=48(apache) groups=48(apache) context=system_u:system_r:httpd_t:s0，看到最后的context=system_u:system_r:httpd_t:s0没，就是SELinux在作祟。





