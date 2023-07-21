# Nginx alias vs root

他们都是 nginx 虚拟目录配置，不同的是他们处理路径的方式。

## Introduction

### alias
[官方文档](http://nginx.org/en/docs/http/ngx_http_core_module.html#alias)
> Defines a replacement for the specified location. 

location 部分会替换 path，所以 `final path = alias`

### root
[官方文档](https://nginx.org/en/docs/http/ngx_http_core_module.html#root)
> Sets the root directory for requests. A path to the file is constructed by merely adding a URI to the value of the root directive. If a URI has to be modified, the alias directive should be used.

location 部分会添加到 path 上，所以 `final path = root + location`

## Examples

```shell
location /img/ {
    alias /var/www/image/;
}
# 访问/img/目录里面的文件时，ningx会自动去/var/www/image/目录找文件
location /img/ {
    root /var/www/image;
}
# 访问/img/目录下的文件时，nginx会去/var/www/image/img/目录下找文件。]
```

## 参考资源
- [nginx-tutorial](https://wangchujiang.com/nginx-tutorial/)
- [nginx-static-file-serving-confusion-with-root-alias](https://stackoverflow.com/questions/10631933/nginx-static-file-serving-confusion-with-root-alias)

