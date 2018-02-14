---
title: Setting Proxy server
keywords: howto proxy 
tags: [quickstart, getting_started]
sidebar: home_sidebar
summary : Explanation about setting proxy server
---

I would like to explain about how to set sites using reverse proxy and load balancer.

By this setting, requests of EC-Cube will be treated as requests from reverse proxy and load balancer.

EC-CUBEのセキュリティオプションで「サイトへのアクセスを、SSL（https）経由に制限します」を有効してリダイレクトループが発生する場合は、この設定を行うことで回避できるようになります。


![サイト構成の例](/images/proxy_settings/network_diagram.png)  

## Objective
From EC-CUBE 3.0.14

## How to set when installing

インストールの「サイトの設定」ページ  
`オプションを表示 > ロードバランサー、プロキシサーバの設定` で、Proxyサーバーの設定ができます。

![インストーラのオプション](/images/proxy_settings/install_options.png)  

サイトが信頼されたロードバランサー、プロキシサーバからのみアクセスされる  
- このオプションをONにした場合、全てのリクエストを信頼されるプロキシサーバからのリクエストとして扱うようになります。  
- 信頼されるプロキシサーバ以外からのトラフィックを防いだサーバー環境でのみ、このオプションを利用してください。
    
ロードバランサー、プロキシサーバのIP  
- 個別にプロキシサーバを指定する場合は、ここのIPアドレスを入力してください。  
- サブネットマスクを利用したCIDR表記も可能です。

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
- 「サイトが信頼されたロードバランサー、プロキシサーバからのみアクセスされる」オプション。

trusted_proxies
- 「ロードバランサー、プロキシサーバのIP」オプション。
- EC-CUBE自身からのサブリクエストを処理する必要があるため、ローカルループバックアドレスは削除しないでください。

## Restriction

EC-CUBEがプロキシサーバからのリクエストを適切に処理するためには、プロキシサーバがリクエストに`X-Forwarded-*`ヘッダーを付与している必要があります。

## Reference

Symfony - How to Configure Symfony to Work behind a Load Balancer or a Reverse Proxy  
[http://symfony.com/doc/2.7/request/load_balancer_reverse_proxy.html](http://symfony.com/doc/2.7/request/load_balancer_reverse_proxy.html)

## 補足：さくらのレンタルサーバでの設定について

さくらのレンタルサーバーでSNI SSLを利用した場合、「https://」についてはプロキシとして動作しますが、制限事項にある`X-Forwarded-*`ヘッダーが付与されないため上記設定をしてもリダイレクトループが発生してしまいす。

html/.htaccess にある下記のコードを有効化して対処してください。

```.htaccess
# さくらのレンタルサーバでサイトへのアクセスをSSL経由に制限する場合の対応
RewriteCond %{HTTP:x-sakura-forwarded-for} !^$
RewriteRule ^(.*) - [E=HTTPS:on]
```
