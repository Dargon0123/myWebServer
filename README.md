Raw_version配置文档:jack_o_lantern:
===============
## 简介:key:

学习Linux网络编程，根据游双大佬的[《Linux高性能服务器编程》](https://book.douban.com/subject/24722611/)，实践其中的例程，实现一个`Linux下基于C++11 的多线程轻量级WebServer`

* 使用**线程池 + epoll(ET和LT均实现) + 模拟Proactor模式**的并发模型
* 使用**状态机**解析HTTP请求报文，支持解析**GET和POST**请求
* 通过访问服务器数据库实现web端用户**注册、登录**功能，可以请求服务器**图片和视频文件**
* 实现**同步/异步日志系统**，记录服务器运行状态
* 经Webbench压力测试可以实现**上万的并发连接**数据交换

基础测试:astonished:
------------
* 服务器测试环境
	* Ubuntu版本20.04
	* MySQL版本5.7.29
	
* 浏览器测试环境
	* Linux
	* Chrome
	
* 测试前确认已安装MySQL数据库

    ```C++
    // 建立yourdb库
    create database yourdb;

    // 创建user表
    USE yourdb;
    CREATE TABLE user(
        username char(50) NULL,
        passwd char(50) NULL
    )ENGINE=InnoDB;

    // 添加数据
    INSERT INTO user(username, passwd) VALUES('name', 'passwd');
    ```

* 修改main.c中的数据库初始化信息

    ```C++
    // root root修改为服务器数据库的登录名和密码
	// qgydb修改为上述创建的yourdb库名
    connPool->init("localhost", "root", "root", "yourdb", 3306, 8);
    ```

* 修改http_conn.cpp中的root路径

    ```C++
	// 修改为root文件夹所在路径
    const char* doc_root="/home/{your home]/TinyWebServer/root";
    ```

* 生成server

    ```C++
    make server
    ```

* 启动server

    ```C++
    ./server port # 服务器发布一个port，表示一个端口的服务，3306 运行起来！
    ```

* 浏览器端

    ```C++
    ip:port # 浏览器端口输入 192.168.145.128:3306
    ```

## 参考资料:bear:

* [《Linux高性能服务器编程》游双](https://book.douban.com/subject/24722611/)
* [TinyWebServer](https://github.com/qinguoyi/TinyWebServer)