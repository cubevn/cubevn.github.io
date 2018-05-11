---
layout: default
title: Web API Authentication (Authorization) Guide
---

---

# Web API Authentication (Authorization) Guide

## Summary

In EC-CUBE, when excecute Web API, it is not necessary in case refer the general public info. 
But it is necessary for authentication in case refer the customer info or update the receiving order info.

In EC-CUBE 3, supporting Authentication that used  [OpenID Connect](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html)
In case you use this authentication, need [TLS をサポート](http://openid-foundation-japan.github.io/openid-connect-basic-1_0.ja.html#TLSRequirements)

## The handling Flow

- [Authorization Code Flow](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#CodeFlowAuth) - For Web App
- [Implicit Flow](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#ImplicitFlowAuth) - For JavaScript, Native App 

In case you use in Private environment that secured reliability, you can also use [OAuth2.0 Authorization](http://openid-foundation-japan.github.io/rfc6749.ja.html)

## Setting method

### Implemenation of Administrator Authority

1. Login in Management screen, move to メンバー管理画面(member management screen)
2. Click **APIクライアント一覧**(API Client list), register new API client
    -Input the optional name for **アプリケーション名** 
    -In **redirect_uri**, input URL of direct destination from Authorization Endpoint
3. If the registration has finished, `client_id`, `client_secret` will be issued. The public key will use when verify `id_token`.

Implement by ### 顧客(Customer)

1. Login in mypage, access to `/mypage/api` 
2. Click **新規登録**, register new API client
    - Input the optional name for **アプリケーション名**
    -In **redirect_uri**,  input URL of direct destination from Authorization Endpoint
3. If the registration has finished, `client_id`, `client_secret`will be issued. The public key will use when verify `id_token`.

Setting of ### .htaccess 

In environment of the part of rental server or SAPI CGI/FastCGI, sometimes you can not get authentication info (Authorization header) and get errror 401 Unauthorized
In this case, please add the follwing part into `<ec-cube-install-path>/html/.htaccess`

```.htaccess
RewriteCond %{HTTP:Authorization} ^(.*)
RewriteRule ^(.*) - [E=HTTP_AUTHORIZATION:%1]
```

## The implementation method of Client (OAuth 2.0 Client/Relying Party)

*Sample of each Language is [こちら](/api.html#section-14)*

### Notice when implement

#### state parameter

In OAuth2.0, state parameter using for preventing CSRF, is [推奨となっています](http://openid-foundation-japan.github.io/rfc6749.ja.html#CSRF)(recommendation)
However, samples of many OAuth2.0 Clients have not handled for state paramter in standard,
In EC-CUBE 3, **state パラメータは必須**(state parameter is required), so please pay attention.

### Implementation Tutorial

Here, based on command curl to try implementing OpenID Connect Authorization code Flow

#### Preparation

[APIクライアントを作成](#section-2)(Create API client)

In this example, set `redirect_uri` in `https://127.0.0.1:8080/Callback`
#### 1. Acquisition of Authorization code 

Access into the following URL by Browser. **state パラメータは必須** (state parameter is required)

```
https://<eccube-host>/admin/OAuth2/v0/authorize?client_id=<client id>&redirect_uri=http%3A%2F%2F127.0.0.1%3A8080%2FCallback&response_type=code&state=<random_state>&nonce=<random_nonce>&scope=product_read%20product_write%20openid%20offline_access
```

Login screen is display, so Login 
Because screen 「このアプリ連携を許可しますか？」(permit linking this App?), 
If click 「許可する」(permit), will direct to the specified Redirect destination.

This time, `code=<authorization code>` is given in Query string of Address bar of Browser
In order to prevent CSRF, confirm whether value of  **state** before redirect and
value of `state=<state>` that was given in Query string of Address bar are same or not?

#### 2. Acquisition of Access token 

Use Authorization code that got at 1 in order to get Access token.

```
curl -F grant_type=authorization_code \
     -F code=<authorization code> \
     -F client_id=<client id> \
     -F client_secret=<client secret> \
     -F state=<random_state> \
     -F nonce=<random_nonce> \
     -F redirect_uri=http%3A%2F%2F127.0.0.1%3A8080%2FCallback \
     -X POST https://<eccube-host>/OAuth2/v0/token
```

It is possibe to get the following Access token and Refresh token, `id_token`

```
{
  "access_token":"e5b0a9a885eb2a5a4aacff3b3a11596e346b9703",
  "expires_in":3600,
  "token_type":"bearer",
  "scope":"product_read product_write openid offline_access",
  "refresh_token":"0e3f3741514240f48d180f3cbf03d53410f064ef",
  "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xOTIuMTY4LjU2LjEwMTo4MDgxIiwic3ViIjoiNG53c25ic3pJSDRYdVFuWHdpZ3RQOEhhS1FwamVHeDQ5OXMwQTlJMXFrbyIsImF1ZCI6IjJlODE1MzAyM2Q2YWZiMmZiMjk5MzFkYmY5YTI3NWVkNDcxNWYzODQiLCJpYXQiOjE0NjA1MzU1MjMsImV4cCI6MTQ2MDUzOTEyMywiYXV0aF90aW1lIjoxNDYwNTM1NTIzLCJub25jZSI6InJhbmRvbV9ub25jZSJ9.D3RE1i-Oc_bCANI28BwqT-6voLk645kqGZCs3PCOfDRATUX6_hvyBOc3PvfrH6BCaNfYX8m8sGQPD2g-GRUJ-j6OMCHp1KHcycsN5OS6QoZOucvM_gDKITivwW0q3BvLYsc-zK00DRlYuAhSW1pCqdWGRGk-3LWbqfasttYvx34KoSazfCsIyMqxC_zQ4qDoYaReeuCjiMX1xW3vXueEidMQ9_5s7SQgJwtwMnqOdDoEHUQce65wWa2yNXBHaohrGwXmg9Sbd5pD_Anhrh7WIAnYEbDoHc1rb40oUT-kye5cplYUTd4F9y88PnyXeWN3-vGRVxsvMRdJQmiTqzwVvA"
}
```

#### 3. Verification of id_token

[検証](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#SelfIssuedValidation)(verify)`id_token` that got in 2

It is possibe to use [tokeninfo](#tokeninfo) End-point and [jwt.io](https://jwt.io/).

#### 4. API Access

Use Access token to Access API

```
curl -H "Accept: application/json" \
     -H "Content-type: application/json" \
     -H 'Authorization: Bearer <access token>' \
     -X GET https://<eccube-host>/api/v0/product
```

#### 5. Update Access token 

In case valid time of Access Token  expired, use Refresh token to update Acess token.
In case of `scope=openid`, it is neccessary to add `offline_access` into scope.

```
curl -F grant_type=refresh_token \
     -F refresh_token=<refresh token> \
     -F client_id=<client id> \
     -F client_secret=<client secret> \
     -X POST https://<eccube-host>/OAuth2/v0/token
```

## About Implementations

### Things which EC-CUBE's own has been implementing

#### tokeninfo

If you request GET to `/OAuth2/tokeninfo?id_token=<id_token>`, you can get the detail info of `id_token` by JSON format

```json
{
  "iss":"https:\/\/example.com",
  "sub":"4nwsnbszIH4XuQnXwigtP8HaKQpjeGx499s0A9I1qko",
  "aud":"2e8153023d6afb2fb29931dbf9a275ed4715f384",
  "iat":1460535523,
  "exp":1460539123,
  "auth_time":1460535523,
  "nonce":"random_nonce"
}
```

- **iss** - The issue source of ID token
- **sub** - User identifier. It is generated from public key of `id_token`
- **aud** - Audience that is assumed of token. OAuth2.0 Client ID is used.
- **iat** - UNIX Time Stamp of  ID token issue time.
- **exp** - UNIX Time Stamp of ID token valid time
- **auth_time** - UNIX Time Stamp of time when occurred authentication.
- **nonce** - Identifier of Client session. Use for preventing Replacement Attack

You need based on this info to verify the following contents [参考](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#SelfIssuedValidation)

- Check that value of **iss** is match with Host name that authenticated API
- Check that value of **sub** is match with *thumbprint* of public key of `id_token`. It is possible to verify such as  [JOSE_JWK::thumbprint()](https://github.com/gree/jose/blob/master/src/JOSE/JWK.php#L35) 
- Check that value of **aud**  is match with Client ID
- Check that value of  **iat** is over `現在のUNIXタイムスタンプ値 - 600秒` (current UNIX Time stamp value - 600 seconds) 
- Check that value of **exp** is bigger than UNIX Time stamp value of current time
- Check that value of **nonce** is same with value of `nonce` which is holding in Client. In order to prevent Replace Attack, dsicard `nonce` which is stored in session.

#### Relation between Member/Customer and OAuth2.0 Client 

- In case Member/Customer and OAuth2.0 Client are different when logging in, when request authorization, it will be error  `access_denied`.

#### Specification of redirect_uri

In Authorization Code Flow, by specifying  `redirect_uri` に `urn:ietf:wg:oauth:2.0:oob`,Authorization code is displayed in screen of Browser. It is possible to use for Test purpose that used curl, and Native Application.

### Things that base on standard specification

#### ID Token

Using [RFC7519 JSON Web Token](http://openid-foundation-japan.github.io/draft-ietf-oauth-json-web-token-11.ja.html)

#### OAuth2.0 Authorization Code Flow

Using [RFC6749 Authorization Code Grant](http://openid-foundation-japan.github.io/rfc6749.ja.html#grant-code) 

- In this API, **state パラメータは必須**  (state paramater is required)

#### OAuth2.0 Implicit Code Flow

Using [RFC6749 Implicit Grant](http://openid-foundation-japan.github.io/rfc6749.ja.html#grant-implicit) を使用しています。

- In this API,**state パラメータは必須**  (state paramater is required)

#### OpenID Connect Authorization Code Flow

Using [OpenID Connect Core Authorization Code Flow](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#CodeFlowAuth) 

- In this API, **state パラメータは必須** (state paramater is required)

#### OpenID Connect Implicit Code Flow

Using [OpenID Connect Core Implicit Code Flow](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#ImplicitFlowAuth) 

- In this API, **state パラメータは必須** (state paramater is required)
- In case of `response_type=id_token and  response_type=id_token token`, **nonce パラメータは必須** (nonce parameter is required)

#### UserInfo Endpoint

Using [OpenID Connect UserInfo Endpoint](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#UserInfo) 

- In case using this Endpoint, you need authentication by `scope=openid` 
- You can use the following scope to get [各種クレーム](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#Claims)(claims of each kind)
  - profile
  - email
  - address
  - phone
