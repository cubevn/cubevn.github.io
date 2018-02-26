---
title: Setting Proxy server
keywords: howto proxy 
tags: [quickstart, getting_started]
sidebar: home_sidebar
summary : Explanation about setting proxy server
---

I would like to explain about how to configure a site using Reverse proxy and Load balancer.

By this setting, requests to EC-Cube will be treated as requests from reverse proxy and load balancer.

You also can prevent redirect loop when enable a option 「サイトへのアクセスを、SSL（https）経由に制限します」(Meaning: Limit access to site via SSL (https)) in EC-CUBE 


![サイト構成の例](/images/proxy_settings/network_diagram.png)  

## Objective
From EC-CUBE 3.0.14

## How to set when installing

When install, go to page 「サイトの設定」
and set Proxy server in `オプションを表示 > ロードバランサー、プロキシサーバの設定` 

![インストーラのオプション](/images/proxy_settings/install_options.png)  

Site allow only access trusted by Load balancer ans Proxy server.  
- If it is ON mode, all requests will be treated as requests from reliable proxy server.
- Please use this option only when in a server environment which preventing traffic Id not trusted by proxy server.
    
IP of proxy server and load balancer
- To specify a proxy server individually, enter the IP address here.  
- It is possible to display CIDR using subnet mask.

## config.yml

After installing, we can change by editing confif.yml

```yaml
[app/config/eccube/config.yml]

trusted_proxies_connection_only: true
trusted_proxies:
    - 127.0.0.1/8
    - '::1'
```

trusted_proxies_connection_only
- Option「サイトが信頼されたロードバランサー、プロキシサーバからのみアクセスされる」(Meaning: Site allow only access trusted by load balancer ans proxy server)

trusted_proxies
- Option「ロードバランサー、プロキシサーバのIP」(Meaning: IP of load balancer and proxy server)
- Please do not delete the local loopback address because it is necessary to process the sub request from EC-CUBE.

## Restriction

In order for EC-CUBE to properly handle requests from proxy servers, the proxy server have to add `X-Forwarded-*` to header.

## Reference

Symfony - How to Configure Symfony to Work behind a Load Balancer or a Reverse Proxy  
[http://symfony.com/doc/2.7/request/load_balancer_reverse_proxy.html](http://symfony.com/doc/2.7/request/load_balancer_reverse_proxy.html)

## Additional: About setting in Sakura rental server

If you use SNI SSL in Sakura rental server, 「https://」 will become a proxy. But because `X-Forwarded-*`in Restriction is not included in header, redirect loop still happen.

Please enable these codes below to handle.

```.htaccess
# さくらのレンタルサーバでサイトへのアクセスをSSL経由に制限する場合の対応
RewriteCond %{HTTP:x-sakura-forwarded-for} !^$
RewriteRule ^(.*) - [E=HTTPS:on]
```
