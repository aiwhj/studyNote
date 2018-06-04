1. prefork 中没有线程的概念，是多进程模型，一个进程处理一个连接；稳定；响应快。其缺点是在连接数比较大时就非常消耗内存。

2. worker 是多进程多线程模型，一个进程有多个线程，每个线程处理一个连接。与prefork相比，worker模式更节省系统的内存资源。不过，需要注意worker模式下的Apache与php等程序模块的兼容性。

3. event 是worker模式的变种，它把服务进程从连接中分离出来,在开启KeepAlive的场合下相对worker模式能够承受更高的并发负载,不能很好的支持https的访问

4. apache使用mod_php的话,不能使用worker模式,不是线程安全的

5. worker模式 可以使用 mod_fastcgi 连接 php-fpm



Apache mpm prefwork + mod_php
Apache mpm worker + mod_fcgid
Apache mpm worker + fastcgi + php-fpm
Nginx + fastcgi + php-fpm




via[https://coolpandaca.wordpress.com/2012/03/20/apache-mpm-worker-prefork-mod_php-mod_fcgid-mod_fastcgi-php-fpm-and-nginx/](https://coolpandaca.wordpress.com/2012/03/20/apache-mpm-worker-prefork-mod_php-mod_fcgid-mod_fastcgi-php-fpm-and-nginx/)