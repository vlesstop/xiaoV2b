# xiaoV2b
本教程是基于@wyx2685 的v2board ，支持webman后的版本。 
https://github.com/wyx2685/v2board/
-   [v2board]([[https://github.com/InazumaV/V2bX](https://github.com/wyx2685/v2board)](https://github.com/wyx2685/v2board))

## 第一步首先安装aapanel /宝塔，这一步不会的不建议继续操作了，如果实在想研究的 可自行搜索 安装教程
![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/1f00a9a1-79f5-4379-9758-6f9527f67e1f)

## 安装完成后登录宝塔，选择安装如下
☑️ Nginx 1.2.2
☑️ MySQL 5.7
☑️ PHP 8.1
☑️ phpmyadmin-5.2

![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/f069014e-5595-4ea9-8695-600bc04845b3) 

等待安装完毕
预计耗时：10-30分钟之间

如下图就是已经安装完成


![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/eec0491e-207c-41c9-954a-61a36d7610de)


## 安装Redis、fileinfo
 宝塔 面板 > 软件商店 > 找到PHP 8.1 点击Setting > Install extentions > redis,fileinfo 进行安装。
预计耗时：5分钟

 ![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/0a7b977d-3e52-4cd8-9277-66b0dab78734)

![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/7c84589e-fb85-4bb7-8f50-8c22e32a604e)

 

## 解除被禁止的函数
进行这一步之前，建议要等到 redis,fileinfo 安装完成之后再进行操作。否则可能在安装依赖试提示未禁用 某些函数

宝塔 面板 > 软件商店 > 找到PHP 8.1   点击Setting > Disabled functions 将 putenv 、 proc_open、  pcntl_alarm 、pcntl_signal 从列表中删除。
![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/6d698912-2f76-44de-8b38-006521d7a444)



## 添加站点
宝塔 面板 > 网站 > 添加站点

在 域名 填入你指向服务器的域名
在 Database 选择MySQL
在 PHP Verison 选择PHP-8.1
![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/4ffcbbac-85ff-4b22-9aae-21b13082a10c)

## 登录到SSH 进行下面的操作



     git clone https://github.com/wyx2685/v2board.git ./
## 克隆完成后执行以下命令
      sh init.sh

![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/2e28d801-5db1-4074-ae31-a25180442278)

![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/1da7e5cc-136a-49e4-9e96-bc3937e22e09)


## 配置站点目录及伪静态

返回到宝塔页面， 选择网站， 点击 网站名，选择网站目录
![image](https://github.com/vlesstop/xiaoV2b/assets/48223192/eed3dafa-d85e-42a5-b625-5f6d746fbbb3)


## 然后选择伪静态，填写以下内容
```
location /downloads {
}

location / {
            try_files $uri $uri/ @backend;
        }

        location ~ (/config/|/manage/|/webhook) {
            try_files $uri $uri/ /index.php$is_args$query_string;
        }

        location @backend {
            proxy_pass http://127.0.0.1:6600;
        }

location ~ .*\.(js|css)?$
{
    expires      1h;
    error_log off;
    access_log /dev/null; 
}
```

