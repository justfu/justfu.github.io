---
layout: post
title: "基于php使用memcache存储session的详解"
keywords: ["session"]
description: "基于php使用memcache存储session的详解"
category: "session"
tags: ["php"]
---
{% include JB/setup %}
![images](https://img.alicdn.com/imgextra/i3/1819728314/TB2.DCJeVXXXXavXXXXXXXXXXXX_!!1819728314.jpg)

web服务器的php session都给memcached ，这样你不管分发器把 ip连接分给哪个web服务器都不会有问题了，配置方法很简单，就在php的配置文件内
增加一条语句就可以了，不过前提你需要装好memcache模块

###### 设置session用memcache来存储
方法I: 在 php.ini 中全局设置
```
session.save_handler = memcache
session.save_path = "tcp://127.0.0.1:11211"
```

方法II： 某个目录下的 .htaccess ：
```
php_value session.save_handler "memcache"
php_value session.save_path  "tcp://127.0.0.1:11211"
```

方法III： 再或者在某个一个应用中：
```
ini_set("session.save_handler", "memcache");
ini_set("session.save_path", "tcp://127.0.0.1:11211");
```

使用多个 memcached server 时用逗号","隔开，并且和 Memcache::addServer() 文档中说明的一样，可以带额外的参数"persistent"、"weight"、"timeout"、"retry_interval" 等等，类似这样的："tcp://host1:port1?persistent=1&weight=2,tcp://host2:port2" 。
如果安装的PECL是memcached(使用libmemcache库的那个)，则配置应为
```
ini_set("session.save_handler", "memcached"); // 是memcached不是memcache
ini_set("session.save_path", "127.0.0.1:11211"); // 不要tcp:
```

########  启动 memcached：
memcached -d -l 127.0.0.1 -p 11212 -m 128


或 启动Memcache的服务器端：

```
memcached -d -m 100 -u root -l 192.168.36.200 -p 11211 -c 256 -P /tmp/memcached.pid  
# /usr/local/bin/memcached -d -m 10 -u root -l 192.168.0.200 -p 12000 -c 256 -P /tmp/memcached.pid
```

引用
    -d选项是启动一个守护进程，
    -m是分配给Memcache使用的内存数量，单位是MB，我这里是100MB，
    -u是运行Memcache的用户，我这里是root，
    -l是监听的服务器IP地址，如果有多个地址的话，我这里指定了服务器的IP地址192.168.36.200，
    -p是设置Memcache监听的端口，我这里设置了11211，最好是1024以上的端口，我们这里统一使用11211
    -c选项是最大运行的并发连接数，默认是1024，我这里设置了256，按照你服务器的负载量来设定。
    -P是设置保存Memcache的pid文件，我这里是保存在/tmp/memcached.pid，
    
###### 在程序中使用 memcache 来作 session 存储
用例子测试一下：
复制代码 代码如下:
```
    <?php  
    session_start();  
    if (!isset($_SESSION['TEST'])) {  
        $_SESSION['TEST'] = time();  
    }  

    $_SESSION['TEST3'] = time();  

    print $_SESSION['TEST'];  
    print "<br><br>";  
    print $_SESSION['TEST3'];  
    print "<br><br>";  
    print session_id();  
    ?>  
```

###### 用 sessionid 去 memcached 里查询一下：
复制代码 代码如下:
```php
    <?php  
      $memcache = memcache_connect('localhost', 11211);  
      var_dump($memcache->get('19216821213c65cedec65b0883238c278eeb573e077'));  
      $memcache->set('aaaa', 'hello everyone');  
      var_dump($memcache->get('aaaa'));  
    ?> 
```

会看到
```
string(37) "TEST|i:1177556731;TEST3|i:1177556881;"
```
这样的输出，证明 `session` 正常工作。
用 `memcache` 来存储` session` 在读写速度上会比 files 时快很多，而且在多个服务器需要共用 session 时会比较方便，将这些服务器都配置成使用同一组 memcached 服务器就可以，减少了额外的工作量。缺点是 session 数据都保存在 memory 中，持久化方面有所欠缺，但对 session 数据来说也不是很大的问题。

===================================
一般地， Session 是以文本文件形式存储在服务器端的。如果使用 Seesion，或者该 PHP 文件要调用 Session 变量，那么就必须在调用 Session 之前启动它，使用 session_start() 函数。其它都不需要你设置了，PHP 自动完成 Session 文件的创建。其默认 Session 的存放路径是服务器的系统临时文件夹。 
但是如果碰到大数据量的Sesstion的时候， 使用基于文件的Session存取瓶颈可能都是在磁盘IO操作上，现在利用Memcached来保存Session数据，直接通过内存的方式，效率自然能够提高不少。 在读写速度上会比 files 时快很多，而且在多个服务器需要共用 session 时会比较方便，将这些服务器都配置成使用同一组 memcached 服务器就可以，减少了额外的工作量。

其缺点是 session 数据都保存在 memory 中，一旦宕机，数据将会丢失。但对 session 数据来说并不是严重的问题。
如何用 memcached 来存储 session呢？以下是基本的配置步骤：
1. 安装 memcached 
在 phpinfo 输出中的 “Registered save handlers” 会有 “files user sqlite”。

2. 修改配置文件，
a. 在 php.ini 中全局设置（* 需要重启服务器）
 ```      
session.save_handler = memcache
session.save_path = "tcp://127.0.0.1:11211"
```
b. 或者某个目录下的 .htaccess :
```
php_value session.save_handler "memcache"
php_value session.save_path "tcp://127.0.0.1:11211"
```
c. 也可以在某个一个应用中：
```
ini_set("session.save_handler", "memcache");
ini_set("session.save_path", "tcp://127.0.0.1:11211");
```
注：使用多个 memcached server 时用逗号”,”隔开，并且和 Memcache::addServer() 文档中说明的一样，可以带额外的参数”persistent”、”weight”、”timeout”、”retry_interval” 等等，类似这样的：”tcp://host:port?persistent=1&weight=2,tcp://host2 :port2″ 。

######  启动 memcached
```
memcached -d -m 10 -u root -l 127.0.0.1 -p 11211 -c 256 -P /tmp/memcached.pid
```
###### 测试 创建一个 session
复制代码 代码如下:
```
    <?php
        //set_session.php
        session_start();
        if (!isset($_SESSION['admin'])) {
        $_SESSION['TEST'] = 'wan';
        }
        print $_SESSION['admin'];
        print "\n";
        print session_id();
    ?>
```

###### 用 sessionid 去 memcached 里查询一下
复制代码 代码如下:
```
    <?php
        //get_session.php
        $mem = new Memcache;
        $mem->connect("127.0.0.1", 11211);
        var_dump($mem->get('0935216dbc0d721d629f89efb89affa 6'));
    ?>
```

复制代码 代码如下:
```
[root@localhost html]# /usr/local/webserver/php/bin/php -f get_session.php
```
------
* 用 memcache 来存储 session 在读写速度上应该会比文件快很多，而且在多个服务器需要共用 session 时会比较方便，将这些服务器都配置成使用同一组 memcached 服务器就可以，减少了额外的工作量。缺点是 session 数据都保存在内存中，不能持久化存储，如果想持久化存储，可以考虑使用Memcachedb来存储，或用Tokyo Tyrant+Tokyo Cabinet来进行存储。
------
* 怎样判断session失效了呢？在php.ini中有个Session.cookie_lifetime的选项，这个代表SessionID在客户端Cookie储存的时间，默认值是“0”，代表浏览器一关闭，SessionID就作废，这样不管保存在Memcached中的Session是否还有效(保存在Memcached中的session会利用Memcached的内部机制进行处理，即使session数据没有失效，而由于客户端的SessionID已经失效，所以这个key基本上不会有机会使用了，利用Memcached的LRU原则，如果Memcached的内存不够用了，新的数据就会取代过期以及最老的未被使用的数据)，因为SessionID已经失效了，所以在客户端会重新生成一个新的SessionID。
------
* 保存在Memcached中的数据最长不会超过30天,这个时间是以操作Memcached的时间为基准的，也就是说，只要key还是原来的key,如果你重新对此key进行了相关的操作(如set操作)，且重新设置了有效期，则此时此key对应的数据的有效期会重新计算的,php手册中有说明
Expiration time of the item. If it's equal to zero, the item will never expire. You can also use Unix timestamp or a number of seconds starting from current time, but in the latter case the number of seconds may not exceed 2592000 (30 days). 
Memcached主要的cache机制是LRU（最近最少用）算法+超时失效。当您存数据到memcached中，可以指定该数据在缓存中可以呆多久。如果memcached的内存不够用了，过期的slabs会优先被替换，接着就轮到最老的未被使用的slabs。
-----

* 为了使web应用能使用saas模式的大规模访问,必须实现应用的集群部署.要实现集群部署主要需要实现session共享机制,使得多台应用服务器之间会话统一, tomcat等多数服务都采用了session复制技术实现session的共享.
session复制技术的问题:
(1)技术复杂,必须在同一种中间件之间完成(如:tomcat-tomcat之间).
(2)在节点持续增多的情况下,session复制带来的性能损失会快速增加.特别是当session中保存了较大的对象,而且对象变化较快时,性能下降更加显著.这种特性使得web应用的水平扩展受到了限制.
-----

* session共享的另一种思路就是把session集中起来管理,首先想到的是采用数据库来集中存储session,但数据库是文件存储相对内存慢了一个数量级,同时这势必加大数据库系统的负担.所以需要一种既速度快又能远程集中存储的服务,所以就想到了memcached.
memcached能缓存什么？
通过在内存里维护一个统一的巨大的hash表，Memcached能够用来存储各种格式的数据，包括图像、视频、文件以及数据库检索的结果等。
memcached快么？
非常快。memcached使用了libevent（如果可以的话，在linux下使用epoll）来均衡任何数量的打开链接，使用非阻塞的网络I/O，对内部对象实现引用计数(因此，针对多样的客户端，对象可以处在多样的状态)， 使用自己的页块分配器和哈希表， 因此虚拟内存不会产生碎片并且虚拟内存分配的时间复杂度可以保证为O(1).。
使用过程注意几个问题和改进思路：
-----
1、memcache的内存应该足够大，这样不会出现用户session从Cache中被清除的问题(可以关闭memcached的对象退出机制)。 
2、如果session的读取比写入要多很多，可以在memcache前再加一个Oscache等本地缓存，减少对memcache的读操作，从而减小网络开销，提高性能。 
3、如果用户非常多，可以使用memcached组，通过set方法中带hashCode，插入到某个memcached服务器 
对于session的清除有几种方案:
(1)可以在凌晨人最少的时候，对memcached做一次清空。(简单)
(2)保存在缓存中的对象设置一个失效时间,通过过滤器获取sessionId的值,定期刷新memcached中的对象.长时间没有被刷新的对象自动被清除.(相对复杂,消耗资源)
-----
