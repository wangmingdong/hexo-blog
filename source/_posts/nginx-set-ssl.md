---
title: Nginx配置SSL证书【转】
date: 2018-11-28 16:59:44
tags: ['nignx', 'ssl']
categories: ['nignx']
---
通过修改简单的Nginx配置文件来实现SSL证书的加持，使得我们的应用程序支持HTTPS访问协议。

### 首先，搞到SSL证书
付费的话就很多选项，我就简单介绍一下免费的吧。 
免费的SSL证书都是针对单一域名，比如baofeidyz.com quan.baofeidyz.com 这两个域名是单独的，所以是需要两个免费的SSL证书。 
腾讯云和阿里云目前都有免费的SSL证书可以申请。 
腾讯云免费SSL证书 
目前来看腾讯云似乎没有直接的入口，需要我们登录到腾讯云的控制台（如下图），请注意我红框选中的地方，我们可以使用SSL证书管理作为我们申请免费SSL证书的入口。如果你找不到这个入口，可以点另外一个红框中的加号添加SSL证书管理。 

![img1](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/1.png)

进入SSL证书管理以后（如下图），点击 申请证书 即可申请免费的SSL证书，剩下的验证环节腾讯云有完善的文档介绍，就不再赘述。

![img2](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/2.png)

__阿里云免费SSL证书__
阿里云的入口比腾讯云的还要难找，而且阿里云的免费SSL证书好像需要人工审核，我已经在腾讯云申请过了，我这里就只是截图演示一下。 
我是从控制台进入的，实际上你不需要进入控制台也可以，主要是需要找到下图中红框圈中的 __CA证书服务（数据安全）__

![img3](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/3.png)

接下来我们直接走购买流程（如下图）

![img4](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/4.png)

接下来就是巧妙的操作，请按照下图中的步骤进行

![img5](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/5.png)

然后我们可以看到选项菜单里面多了一个免费域名的选项，如下图 

![img6](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/6.png)

点选以后，就可以看到右侧的价格变为0了，然后按照流程走下去就可以了。

如果证书在一天内未通过DNS或者文件验证，可以发起工单联系客服解决。

__然后，配置SSL证书到Nginx中__

腾讯云的SSL证书下载包中，有一个单独的Nginx文件夹，里面有两个文件都是我们需要的，如下图所示。

![img7](https://raw.githubusercontent.com/wangmingdong/docImg/master/nignx-set-ssl/7.png)

我们需要把这两个文件放到我们的服务器中，如果是linux系统，推荐放到``/etc/ssl/``目录下

然后我们需要去找到nginx的配置文件。 
关于这个配置文件的路径查找，推荐大家启动nginx后使用``ps -ef | grep nginx``的命令去查找，这样可以很简单的找到真正会生效的那个配置文件。

首先，我们需要启动nginx

``` bash
service nginx start
```

然后再去查找正在运行中的nginx服务

``` bash
ps -ef | grep nginx
```

得到的结果可能是这样的：

``` bash
[root@host ssl]# ps -ef | grep nginx
root      1007     1  0 May20 ?        00:00:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
nginx     2712  1007  0 02:17 ?        00:00:00 nginx: worker process
root      2768  2658  0 04:32 pts/1    00:00:00 grep --color=auto nginx
```

那么我们nginx配置文件的地址就应该是

``` bash
/etc/nginx/nginx.conf
```

使用vim打开配置文件

``` bash
vim /etc/nginx/nginx.conf
```

我们需要在

``` bash
http{

}
```

中去添加一个server节点，如下所示

``` bash
http{
    #http节点中可以添加多个server节点
    server{
        #监听443端口
        listen 443;
        #对应的域名，把baofeidyz.com改成你们自己的域名就可以了
        server_name baofeidyz.com;
        ssl on;
        #从腾讯云获取到的第一个文件的全路径
        ssl_certificate /etc/ssl/1_baofeidyz.com_bundle.crt;
        #从腾讯云获取到的第二个文件的全路径
        ssl_certificate_key /etc/ssl/2_baofeidyz.com.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        #这是我的主页访问地址，因为使用的是静态的html网页，所以直接使用location就可以完成了。
        location / {
                #文件夹
                root /usr/local/service/ROOT;
                #主页文件
                index index.html;
        }
    }

}
```

到这里还不行，因为如果用户使用的是http协议进行访问，那么默认打开的端口是80端口，所以我们需要做一个重定向，我们在上一个代码块的基础上增加一个server节点提供重定向服务。

``` bash
http{
    #http节点中可以添加多个server节点
    server{
        #监听443端口
        listen 443;
        #对应的域名，把baofeidyz.com改成你们自己的域名就可以了
        server_name baofeidyz.com;
        ssl on;
        #从腾讯云获取到的第一个文件的全路径
        ssl_certificate /etc/ssl/1_baofeidyz.com_bundle.crt;
        #从腾讯云获取到的第二个文件的全路径
        ssl_certificate_key /etc/ssl/2_baofeidyz.com.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        #这是我的主页访问地址，因为使用的是静态的html网页，所以直接使用location就可以完成了。
        location / {
                #文件夹
                root /usr/local/service/ROOT;
                #主页文件
                index index.html;
        }
    }
    server{
        listen 80;
        server_name baofeidyz.com;
        rewrite ^/(.*)$ https://baofeidyz.com:443/$1 permanent;
    }

}
```

然后使用保存配置文件，使用``nginx -t``命令对文件对配置文件进行校验，如果看到``successful``表示文件格式证书，这时候我们就可以启动``nginx``服务或者重新加载``nginx``配置文件。 
启动nginx服务：``service nginx start ``
重新加载配置文件：``nginx -s reload``

其实当你在腾讯云下载证书的时候，腾讯云会提供一个链接教你如何配置``nginx``的证书。


--------------------- 
作者：暴沸 
来源：CSDN 
原文：https://blog.csdn.net/baofeidyz/article/details/80435929 
版权声明：本文为博主原创文章，转载请附上博文链接！