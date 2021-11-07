前三步基于这个教程：

[Windows上搭建PHP开发环境_春花无实的记录小站-CSDN博客](https://blog.csdn.net/change_from_now/article/details/102773786)

# nginx1.20.1

nginx -h 查看帮助信息
nginx -v 查看Nginx的版本号
nginx -V 显示Nginx的版本号和编译信息
start nginx 启动Nginx
nginx -s stop 快速停止和关闭Nginx
nginx -s quit 正常停止或关闭Nginx
nginx -s reload 配置文件修改重新加载
nginx -t 测试Nginx配置文件的正确性及配置文件的详细信息
task /fi "imagename eq nginx.exe" windows命令框下查看nginx的进程命令

[更多nginx常用命令及其含义](https://blog.csdn.net/binginsist/article/details/58008995)

* 出现`nginx: [error] CreateFile() "C:\001MyProgramFiles\nginx-1.20.1/logs/nginx.pid" failed (2: The system cannot find the file specified)`？？？如果开着conf/nginx.conf文件的 编辑窗口，要关掉窗口。
* 打不开localhost/index.php？？？不要紧，先安装php。

此时可以打开localhost/index.html（需要开启nginx）

# php7.4.25

此时可以打开localhost/index.php（需要开启nginx，并在php目录运行命令php-cgi.exe -b 127.0.0.1:9000 -c php.ini）

# PhpStorm2021.2.3

[IDEA激活破解教程2021.1 - 哔哩哔哩](https://www.bilibili.com/read/cv11009490#reply5051843353)

[PhpStorm配置PHP环境_未来来-CSDN博客_phpstorm配置php环境](https://blog.csdn.net/tt75281920/article/details/107104311)

这个教程适用于JetBrain家全套产品，因为原理相同。但是要注意时效性。我安装的IDEA是2021.1.3版，PhpStorm是2021.2.3版，实测有效。

需要的插件已保存在我的本地资源库里







基于教程：

[Composer 安装与使用 | 菜鸟教程](https://www.runoob.com/w3cnote/composer-install-and-usage.html)

[Laravel 安装 - Microtiger - 博客园](https://www.cnblogs.com/microtiger/p/12661749.html)

参考：

[windows（WAMP 环境下）安装composer，然后使用composer安装Laravel 5.6版本_Yliku的博客-CSDN博客_使用composer安装laravel](https://blog.csdn.net/qq_41249766/article/details/81121596)

# Composer2.1.11

> Composer是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。

* composer的使用是依赖php路径的，如果你在安装完composer后改动过php路径，那么应该卸载重装composer
* 开启 openssl 配置，我们打开 php 目录下的 php.ini，删掉的不是`;extension=php_openssl.dll`（旧版本是这样） 前面的分号，因为根本不存在这一句。要删分号的是`;extension=php_openssl`

有需要的话可以看看：

[官网（有官方文档）](https://getcomposer.org/)

[Composer 中文网 / Packagist 中国全量镜像（有官方中文文档）](https://www.phpcomposer.com/)

# laravel6.0.2

* 项目目录里没有vendor文件夹？？？请使用composer的 create-project 命令安装 laravel框架。我们要用laravel6.0，所以用命令：`composer create-project --prefer-dist laravel/laravel blog "6.0.*"`
* php artisan --version查到的laravel framework版本不是6.0？？？因为laravel/framework是laravel的主要核心程序，laravel/laravel是为了web程序准备的。除去vendor 目录外，其他的都是属于laravel/laravel 包的。就是说laravel/laravel等于laravel/framework + fzaninotto/faker + mockery/mockery + 等等其他依赖包
  * 安装时的输出摘录：Installing laravel/laravel (v6.0.2): Extracting archive
  * 安装时的输出摘录：Installing laravel/framework (v6.18.43): Extracting archive

有兴趣可以看看：

[php - laravel/laravel vs laravel/framework - Stack Overflow](https://stackoverflow.com/questions/28795182/laravel-laravel-vs-laravel-framework)

[使用命令"php artisan serve"运行Laravel有什么意义? - IT屋-程序员软件开发技术分享社区](https://www.it1352.com/1539869.html)