# nginx

nginx -h 查看帮助信息
nginx -v 查看Nginx的版本号
nginx -V 显示Nginx的版本号和编译信息
start nginx 启动Nginx
nginx -s stop 快速停止和关闭Nginx
nginx -s quit 正常停止或关闭Nginx
nginx -s reload 配置文件修改重新加载
nginx -t 测试Nginx配置文件的正确性及配置文件的详细信息
task /fi "imagename eq nginx.exe" windows命令框下查看nginx的进程命令

[详细配置信息](https://blog.csdn.net/binginsist/article/details/58008995)

[nginx报错[error] CreateFile() failed The system cannot find the file specified_j448749903的博客-CSDN博客](https://blog.csdn.net/j448749903/article/details/107542711)

出现`nginx: [error] CreateFile() "C:\001MyProgramFiles\nginx-1.20.1/logs/nginx.pid" failed (2: The system cannot find the file specified)`？？？如果开着conf/nginx.conf文件的 编辑窗口，要关掉窗口。



localhost/index.html

localhost/index.php（php-cgi.exe -b 127.0.0.1:9000 -c php.ini）