# Nginx location proxy path priority

nginx proxy path 匹配优先级(由高到低)：

1. 精确匹配 `=`

   `location = /uri` 匹配请求URI与 `location` 中的字符串完全相等的请求。

2. 普通字符串匹配

   `location /uri` 匹配请求URI与 `location` 中的字符串开始匹配的请求。

3. 正则表达式匹配 `~`

   `location ~ regex` 匹配请求URI与正则表达式匹配的请求，大小写敏感。

4. 正则表达式匹配 `~*`

   `location ~* regex` 匹配请求URI与正则表达式匹配的请求，大小写不敏感。

5. 匹配到最长的前缀字符串匹配

   `location ^~ /uri` 匹配请求URI与 `location` 中的字符串开始进行前缀匹配的请求，并且不再匹配其他规则。

在 `location` 后面的一个正则表达式 `~`、`~*` 的优先级比 `location` 后面的一个普通字符串匹配规则更高。
所以当 `location` 后面没有 `=` 时，优先匹配正则表达式。