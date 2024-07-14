反向代理配置后，在访问时一直返回502
```nginx
#PROXY-START/bangumi-cover/

location ^~ /bangumi-cover1/
{
    proxy_pass https://lain.bgm.tv/r/400/pic/cover/;
    proxy_set_header Host lain.bgm.tv;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_http_version 1.1;
    # proxy_hide_header Upgrade;

    add_header X-Cache $upstream_cache_status;
		#Set Nginx Cache

    if ( $uri ~* "\.(gif|png|jpg|css|js|woff|woff2)$" )
    {
        expires 1m;
    }
    proxy_ignore_headers Set-Cookie Cache-Control expires;
    proxy_cache cache_one;
    proxy_cache_key $host$uri$is_args$args;
    proxy_cache_valid 200 304 301 302 43200m;
}

#PROXY-END/bangumi-cover/
```

### 查看errorlog
哦哦哦，应该看错误日志
```
tail -f /www/wwwlogs/bgm-test-proxy.sakiko.top.error.log
```
（之前一直看的是/www/server/nginx/logs/error.log，怪不得啥也没有）

报错是这样的
```
2024/07/13 16:24:14 [error] 6058#0: *90048 connect() to [2606:4700:20::681a:917]:443 failed (101: Network is unreachable) while connecting to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://[2606:4700:20::681a:917]:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:14 [error] 6058#0: *90048 SSL_do_handshake() failed (SSL: error:14094410:SSL routines:ssl3_read_bytes:sslv3 alert handshake failure:SSL alert number 40) while SSL handshaking to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://104.26.9.23:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:14 [error] 6058#0: *90048 SSL_do_handshake() failed (SSL: error:14094410:SSL routines:ssl3_read_bytes:sslv3 alert handshake failure:SSL alert number 40) while SSL handshaking to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://104.26.8.23:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:14 [error] 6058#0: *90048 SSL_do_handshake() failed (SSL: error:14094410:SSL routines:ssl3_read_bytes:sslv3 alert handshake failure:SSL alert number 40) while SSL handshaking to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://172.67.73.67:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:14 [error] 6058#0: *90048 connect() to [2606:4700:20::ac43:4943]:443 failed (101: Network is unreachable) while connecting to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://[2606:4700:20::ac43:4943]:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:14 [error] 6058#0: *90048 connect() to [2606:4700:20::681a:817]:443 failed (101: Network is unreachable) while connecting to upstream, client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /bangumi-cover1/l/98/5e/386809_1yR81.jpg HTTP/2.0", upstream: "https://[2606:4700:20::681a:817]:443/r/400/pic/cover/l/98/5e/386809_1yR81.jpg", host: "bgm-test-proxy.sakiko.top"
2024/07/13 16:24:15 [error] 6058#0: *90048 open() "/www/wwwroot/bgm-test-proxy.sakiko.top/favicon.ico" failed (2: No such file or directory), client: 106.34.76.116, server: bgm-test-proxy.sakiko.top, request: "GET /favicon.ico HTTP/2.0", host: "bgm-test-proxy.sakiko.top", referrer: "https://bgm-test-proxy.sakiko.top/bangumi-cover1/l/98/5e/386809_1yR81.jpg"
```

### 问题解决
https://blog.csdn.net/weixin_42612454/article/details/128203273
https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_server_name
```
location ^~ /bangumi-cover1/
{
    proxy_pass https://lain.bgm.tv/r/400/pic/cover/;
    
    proxy_ssl_server_name on;
	# proxy_ssl_session_reuse off; # 可以不加
```
根据博客，加了这两条，完美解决了
> 告诉 Nginx 在与上游服务器进行 SSL 握手时发送服务器名称（SNI，Server Name Indication）
> SNI 是一个扩展，允许一个服务器主机多个 SSL 证书，每个证书对应一个不同的域名。这在使用基于域名的虚拟主机时非常有用。
> 好像这就指的是cloudflare，bangumi的图片也是用cloudflare代理的，这样看来，如果要反向代理cloudflare代理的内容，就都需要这样的操作了

试了试，感觉 `proxy_ssl_session_reuse off;` 可以不加，是关于重用SSL会话的，默认是on，好像不会因为 `proxy_ssl_server_name on;` 而出问题。

```
原图
https://lain.bgm.tv/r/400/pic/cover/l/98/5e/386809_1yR81.jpg
代理后
https://bgm-test-proxy.sakiko.top/bangumi-cover1/l/98/5e/386809_1yR81.jpg
```


