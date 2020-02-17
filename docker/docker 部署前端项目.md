## docker 部署前端项目

Dockerfile

```
#容器内安装 nginx
FROM nginx

#移除 nginx 的 default.conf
RUN rm /etc/nginx/conf.d/default.conf

#把配置好的 nginx 配置文件添加到/etc/nginx/conf.d/ 目录下
ADD default.conf /etc/nginx/conf.d/

#把前端 dist 文件夹复制到 /usr/share/nginx/html 文件夹下
COPY dist/ /usr/share/nginx/html/

#暴露容器内配置的 nginx 端口
EXPOSE 80
```



Nginx.conf

只需要配置server即可

```
server {
    listen 80 default_server;
    server_name _;
    
    location  / {
      root /usr/share/nginx/html;

      index  index.html ;

      try_files $uri $uri/ /index.html;
    }
}
```

