---
layout: default
title: Web API β version Plugin startup Guide
---

---

# EC-CUBE API β version Plugin Startup Guide

## Contents of this chapter

- EC-CUBE API β version
    - Install method
    - Operating confirmation method that used Swagger
    - Document that needs for developing

- Target
	- Based on OAuth2.0 and OpenID Connect to assume about the person who has experience in using authentication /API   
	（Create liking App of Facebook/Twitter/Google)

- Revelant page  
	[API開発指針](/api.html)
	[Web API Authorization ガイド](/api_authorization.html)  

## β version  Basic requirement

1. Can CRUD all tables
    - DELETE is just table which exists del_flg
    - Table that can access public, Table, field that can refer/ updae when authenticate Customer will decide separately.
    - API definition depends on Table structure
    - The hashed data will use as character string
1. β version is testing implementation, so perhaps spec will change until official release. 
	- Name of Endpoint
	- Getable data structure (depend on Table Layout in EC-CUBE)
	- Access limitation to each Table
	- etc..
1. Authentication base on OAuth2.0/OpenID Connect 
    - There us spec which un-support one part (Plan that supports later)
    - There is parameter which is changing into 「推奨→必須」(recommendation ->required) in spec of OpenID Connect in order to strenghthen security.
    - Safety of Authentication depends on Plugin mechanism of SymfonySecurity, php-oauth2-server, EC-CUBE 3.0.
1. Do not support for Single sign-on function.
1. swagger-ui that using for generating API document will use things that has not release of  master branch （SHA:b856d6c）
1. Confirm operation in over PHP5.4
1. Confirm operation in PostgreSQL9.2, MySQL5.5, SQLite3 
1. <a href="https://github.com/EC-CUBE/eccube-api/issues" target="_blank">現状把握している課題(Issue)</a>

## Installation

Please get from EC-CUBE Owner's store.  
  
<a href="http://www.ec-cube.net/products/detail.php?product_id=1116" target="_blank">プラグインダウンロードページ</a>  
  
※β version is trial release, so please **本番環境にはインストールしない** (do not install in real environment)

Access to API End-Point that can access ## public

- After Install EC-CUBE APIβ version Plugin, access into the following URL to get product info 

	URL： http://<サイトURL> /api/v0/product  
	Response format : JSON format

    ※AAbout list of API  that can acess public, please refer `APIエンドポイント一覧` of this chapter.

Access to API End-point that needs for authentication ##

In case use API Endpoint that can not access public, registration and authentication of client are essential.   
In this document, use Swagger as Client and conduct operating confirmation.  
Express order as below

### 1. Register API Client 	

It is necessary to register Client when conduct authentication.  
Execute `APIクライアントの追加`(add API client) from 管理画面(management screen)>設定(setting)>メンバー管理(member management)

---

![EC-CUBE管理API登録画面1](/images/img-webapi-b-register-client-1.png)

---

![EC-CUBE管理API登録画面2](/images/img-webapi-b-register-client-2.png)

---

Click button 登録(register) and finish registration of Client.
In this document, please set Item of [redirect_uri] as explanation of lowest part of Registration screen in order to confirm operation by using `Swagger`.

---

![EC-CUBE管理画面Swaggerリダイレクト](/images/img-webapi-b-swgger-redirect.png)

---

### 2. Confirm operation which used Swagger as Client

In EC-CUBE 3.0, supporting `OAuth2.0 Authorization` and `OpenID Connect` as authentication method.  
 Use `Swagger` to conduct operation confirmation as Client who handled for this authentication.
If click [「APIドキュメントを開く」(open API document), can access to screen `Swagger`
 
---

![EC-CUBE管理画面Swaggerリンク](/images/img-webapi-b-swgger-access.png)

---

### 3. Authentication order

1. Click button 「Authorize」on upper right-hand.
1. Set checkbox of scope that want to use, into ON and click button 「Authorize」in child screen.  
Because move to 管理画面(management screen) of EC-CUBE, so if choose 「許可する」(permit) here, you can access to that scope.  
(Internal will become status got access token)
 

---

![Swagger認証](/images/img-webapi-b-swgger-authorize.png)

---

![Swagger認証コールバック](/images/img-webapi-b-swgger-authorize-callback.png)

---

- Notice
    - Regardless of the scope that chose in Swagger side, Request is executing with all scope, but this is due to bug Swagger. It is not bug of authentication of EC-CUBE API.

### 4. GET(acqusition) the product info
1. Choose 「GET /api/v0/product」
1. If click 「実際に実行」(run actually), you can get the product info.

---

![SwaggerGetSample](/images/img-webapi-b-swgger-get-sample.png)

---
 
### 5. POST(create) the product info
1. Choose 「POST /api/v0/product」
1. Sample of parameter is displayed in ① part of screen.
1. If click 画面の① part of screen, sample is inserted in ② part.
1. If click「実際に実行」(actually run), you can create the product info

---

![SwaggerPostSample](/images/img-webapi-b-swgger-post-sample.png)

---


If you GET（acquire）the product info again, you can add the product info  
  
In case Response code is 401, authentication is failed.  
In case re-do authentication, but it doesn't run well, please check troubleshooting of this chapter.  
 
## About authentication flow of Client

In order to access into API that need authentication of EC-CUBE 3.0, you need itto implement authentication flow of OAuth2.0, OpenID Connect for client.
About detail, please confirm the following document.  
[Web API認証 ( Authorization ) ガイド](http://ec-cube.github.io/api_authorization.html)

### Implementation sample
<a href="https://github.com/nanasess/eccube3-oauth2-client" target="_blank">PHP(Symfony2) での実装例</a>  
<a href="https://github.com/nanasess/eccube3-oauth2-client-for-python" target="_blank">Python(Flask) での実装例</a>  
<a href="https://github.com/nanasess/eccube3-oauth2-client-for-nodejs" target="_blank">Node.js(Express) での実装例</a>  
<a href="https://github.com/nanasess/DotNetOpenAuth" target="_blank">C# での実装例(Web/Wpf)</a>  
<a href="https://github.com/nanasess/eccube3-oauth2-client-for-java" target="_blank">Java での実装例</a>  
<a href="https://developers.google.com/oauthplayground/" target="_blank">Google OAuth 2.0 Playground</a>  	
    - Confirmed operation already in OAuth 2.0 Configuration -> OAuth endpoint -> Custom   
    - It is necessary to give  ```?state=<random_state>``` for Authorization Endpoint

## About the usable API End-point

In EC-CUBE 3.0, conduct implementation API based on rule of REST.  
Please refer detail at the following document.  

API developing document  
[http://ec-cube.github.io/api](http://ec-cube.github.io/api)  
API End-point list  
[https://github.com/EC-CUBE/ec-cube.github.io/blob/master/documents/api/EC-CUBE_API_Endpoint.pdf](https://github.com/EC-CUBE/ec-cube.github.io/blob/master/documents/api/EC-CUBE_API_Endpoint.pdf)

## About info that can get in API
In β version, supplying CRUD access with each Table of EC-CUBE 3.0  
Therefore, data definition that got from API will depend on Table definition of EC-CUBE 3.0.  
Table definition of EC-CUBE 3.0 will refer the following part.  
  
Table definition of EC-CUBE 3.0  
[https://github.com/EC-CUBE/eccube3-doc/tree/master/ER-D](https://github.com/EC-CUBE/eccube3-doc/tree/master/ER-D)  

## Troubleshooting

1. Authenticated, but response becomes 401

   In environment of the part of rental server or SAPI CGI/FastCGI, sometimes you can not get authentication info (Authorization header) and get errror 401 Unauthorized
   
   In this case, please add the following part into  <ec-cube-install-path>/html/.htaccess

    ```
    RewriteCond %{HTTP:Authorization} ^(.*)
    RewriteRule ^(.*) - [E=HTTP_AUTHORIZATION:%1]
    ```
