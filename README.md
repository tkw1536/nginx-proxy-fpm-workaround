# nginx-proxy fpm workaround

As reported in [nginx-proxy/nginx-proxy#1466](https://github.com/nginx-proxy/nginx-proxy/issues/1466) fpm support for nginx-proxy does not work. 
[@flowl noted](https://github.com/nginx-proxy/nginx-proxy/issues/1466#issuecomment-663845245) that this is because of missing configuration lines.

There are two problems with the existing php-fpm setup, both of which this repository solves:
1. The `SCRIPT_FILENAME` parameter is not passed to fastcgi.
2. Non-php files should be served statically.

This repository is based on [https://github.com/flowl/so6292053](https://github.com/flowl/so6292053) by @flowl. 
However the linked repository requires manually rewriting nginx config and does not survive when a new container is detected to nginx-proxy.

This repository furthermore uses `example.local.guys.wtf` as an example domain. 
This domain points to `127.0.0.1` and `::1` in DNS, meaning it can be used for local testing without any further setup. 

To start this example and verify everything is working:
- `docker-compose up`
- Navigate to [http://example.local.guys.wtf/phpinfo.php](http://example.local.guys.wtf/phpinfo.php). This should show a phpinfo page.
- Navigate to [http://example.local.guys.wtf/static.txt](http://example.local.guys.wtf/static.txt). This should show the static file. 

To understand how this solution works, see [docker-compose.yml](docker-compose.yml). 