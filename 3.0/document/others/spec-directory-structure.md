---
layout: default
title: ディレクトリ・ファイル構成
---

---

# Directory and file structure

### Feature

1. Because Silex framework is used in EC-CUBE 3, we will **Greatly change the Directory structure from system 2**.

1. Based on ** Reference of Symfony2's directory structure**, EC-CUBE 3 will become **Original structure**.

1. In progress of **About public directory, almost the same with system 2**.


### Main directory and the roles

- Folder and directory structure is as below.

1. app : Place what had mainly changed by the environment.
1. html : Folder will become Document Root. Only place which be able to refer from outside directly.
1. src : Place the source which is CORE of EC-CUBE.

In the following, we explain detail descripton of each directory

<!--
```
[root]
├──app  （主に環境によって変更が入るものを配置）
|  ├── cache/
|  |    └──eccube/
|  ├── config/
|  |    └──eccube/
|  |         └── *.yml
|  ├──  log/
|  ├──  template/ （拡張したテンプレート・デザインテンプレート）
|  |    ├──  admin/
|  |    └──  default/
|  └──  .htaccess
|
├── html/ （Document Rootとなるフォルダ。外部から直接参照する物のみ配置)
|   ├── css/
|   ├── js/
|   ├── upload/
|   ├── .htaccess
|   ├── index.php
|   └── robots.txt
|
├── src/
|   └── Eccube/ （EC-CUBEのCOREとなるソースを配置）
|       └── Controller/
|       |   └── *Controller.php
|       ├── ControllerProvider/
|       |   ├── AdminControllerProvider.php
|       |   └── FrontControllerProvider.php
|       ├── Entity/
|       |   └── *.php
|       ├── Plugin/
|       ├── Form/
|       ├── Repository/
|       |   └── *Repository.php
|       ├── Resource/
|       |   └── doctrine/
|       |       └──*.orm.yml
|       |   └── template
|       |       └──*.twig
|       ├── Service/
|       |   └── *Service.php
|       ├── ServiceProvider/
|       |   └── EccubeServiceProvider.php
|       └── Application.php
|
└── vendor/
    ├── */
    └── autoload.php
```
-->

#### Under app

- Arrange files such as **Setting file** or **Log file etc..** but put **Plugin under "Plugin" directory**.

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/spec_directory_structure/directory_app.txt"></script>


<!--
```
[EC-CUBE 3インストールディレクトリ]
├── ■ app（主に環境によって変更が入るものを配置）
│   ├── cache
│   │   └── eccube
│   ├── config
│   │   └── eccube ■設定ファイル
│   │       ├── config.yml
│   │       ├── database.yml
│   │       ├── mail.yml
│   │       └── path.yml
│   ├── console
│   ├── log　■ログファイル
│   ├── Plugin
│   └── template　■拡張したテンプレート・デザインテンプレート
│       ├── admin
│       └── default
├── cache
│   └── plugin
│
・・続く
```
-->

#### Under html

- Become **Public directory**, place **Resource file**(file css and image）

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/spec_directory_structure/directory_html.txt"></script>

<!--
```
・・続き
│
├── ■html（Document Rootとなるフォルダ。外部から直接参照する物のみ配置) 
│   ├── index.php
│   ├── index_dev.php
│   ├── install.php
│   ├── plugin
│   ├── robots.txt
│   ├── template
│   │   ├── admin ■管理画面用リソースファイル
│   │   │   └── assets
│   │   │       ├── css
│   │   │       │   └── *.css
│   │   │       ├── fonts
│   │   │       │   └── WEBフォント
│   │   │       ├── img 
│   │   │       │   └── svg/ico/画像
│   │   │       └── js
│   │   │           ├── *.js（EC-CUBE独自）
│   │   │           └── vendor（ライブラリ）
│   │   │               └── *.js
│   │   ├── default ■フロント画面用リソースファイル
│   │   │   ├── css
│   │   │   │   └── *.css
│   │   │   ├── img
│   │   │   │   ├── common
│   │   │   │   │   └── svg/ico
│   │   │   │   └── top
│   │   │   │       └── 画像
│   │   │   └── js
│   │   │       ├──  *.js（EC-CUBE独自）
│   │   │       └── vendor（ライブラリ）
│   │   │           └── *.js
│   │   └── install ■インストール画面用リソースファイル
│   │       ├── assets
│   │       │   ├── css
│   │       │   │   └── *.css
│   │       │   ├── img 
│   │       │   │   └── svg/画像
│   │       │   └── js
│   │       │       └── *.js（EC-CUBE独自）
│   │       ├── css
│   │       │   └── admin_contents.css (管理画面用共通CSS)
│   │       ├── dist（ライブラリ/BootStrapなど）
│   │       │   ├── css
│   │       │   │   └── *.css
│   │       │   ├── fonts
│   │       │   │   └── WEBフォント
│   │       │   └── js
│   │       │       └── *.js
│   │       └── img
│   │           └── common
│   │               └── favicon.ico
│   ├── upload ■アップロードファイル保存
│   │   ├── save_image
│   │   │   └── 画像
│   │   └── temp_image ■アップロードファイル一時保存
│   ├── user_data
│   └── web.config
│
・・続く
```
-->

#### Under src

- Become **Application main body**, arrange php file and Twig file

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/spec_directory_structure/directory_src.txt"></script>

<!--
```
・・続き
│
├── README.md
├── src
│   └── Eccube ■EC-CUBEのCOREとなるソースを配置
│       ├── Application ■Application.phpの親クラスファイルが格納
│       │   └── ApplicationTrait.php
│       ├── Application.php ★ベースとなるクラス、必ずこのクラスから実行される
│       ├── Command ■Consoleコマンド用クラス群
│       │   └── *.php
│       ├── Common ■定数定義クラス
│       │   └── Constant.php
│       ├── Controller ■コントローラークラス群
│       │   ├── Admin
│       │   │   ├── AdminController.php
│       │   │   └── /*/
│       │   │       └── *.php
│       │   ├── /*/
│       │   │   └── *.php
│       │   ├── *.php
│       │   └── AbstractController.php
│       ├── ControllerProvider ■URLマッピング定義ファイル群
│       │   └── *.php
│       ├── Doctrine ■Doctrine拡張クラス群(特化機能)
│       │   └── /*/
│       │       └── *.php
│       ├── Entity ■DB連携用Entityクラス群
│       │   ├── Master
│       │   │   └── *.php
│       │   ├── *.php
│       │   └── *.php
│       ├── Event ■Formイベント定義用クラス群
│       │   └── *.php
│       ├── EventListener ■イベントリスナー用クラス群
│       │   └── *.php
│       ├── Exception ■業務エラークラス群
│       │   └── *.php
│       ├── Form ■Formタイプ(フォーム定義)クラス群
│       │   ├── DataTransformer
│       │   │   └── *.php
│       │   ├── Extension
│       │   │   └── *.php
│       │   └── Type
│       │       ├── /*/
│       │       │   └── *.php
│       │       ├── *.php
│       │       └── *.php
│       ├── InstallApplication.php
│       ├── Plugin ■プラグイン用Managerrクラス群
│       │   └── AbstractPluginManager.php
│       ├── Repository ■DBアクセス用レポジトリクラス群
│       │   ├── /*/
│       │   │   └── *.php
│       │   ├── *.php
│       │   └── *.php
│       ├── Resource ■doctrine用dcmファイルやtwigファイル等
│       │   ├── config
│       │   │   ├── config.yml.dist
│       │   │   ├── constant.yml.dist
│       │   │   ├── database.yml.dist
│       │   │   ├── database.yml.sqlite3.dist
│       │   │   ├── database.yml.sqlite3-in-memory.dist
│       │   │   ├── log.yml.dist
│       │   │   ├── mail.yml.dist
│       │   │   ├── nav.yml.dist
│       │   │   └── path.yml.dist
│       │   ├── doctrine ■doctrine用テーブルマッピング定義クラス群
│       │   │   ├── *.dcm.yml
│       │   │   ├── /*/
│       │   │   │   └── *.dcm.yml
│       │   │   └── migration ■マイグレーションファイル群
│       │   │       └── VersionYYYYMMDDSSMM.php
│       │   ├── locale ■表示メッセージ定義ファイル群
│       │   │   ├── message.ja.yml
│       │   │   └── validator.ja.yml
│       │   └── template ■Twigファイル群
│       │       ├── admin
│       │       │   ├── default_frame.twig
│       │       │   ├── index.twig
│       │       │   ├── login.twig
│       │       │   ├── login_frame.twig
│       │       │   ├── pager.twig
│       │       │   ├── nav.twig
│       │       │   ├── error.twig
│       │       │   └── /*/
│       │       │       └── *.twig
│       │       ├── default
│       │       │   ├── index.twig
│       │       │   ├── block.twig
│       │       │   ├── default_frame.twig
│       │       │   ├── error.twig
│       │       │   ├── pagination.twig
│       │       │   └── /*/
│       │       │       └── *.twig
│       │       ├── exception
│       │       │   └── *.twig
│       │       └── install
│       │           └── *.twig
│       ├── Security ■パスワードハッシュクラスや権限チェッククラス群
│       │   └── /*/
│       │       └── *.php
│       ├── Service ■サービスクラス群( カート処理等の特化クラス )
│       │   └── *.php
│       ├── ServiceProvider ■DI定義用クラス群
│       │   └── *.php
│       ├── Twig ■Twig拡張用クラス群
│       │   └── /*/
│       │       └── *.php
│       └── Util ■共通関数クラス群
│           └── *.php
└── vendor ■Silex本体・Symfony2コンポーネント群・Doctrine・PHPUnitなど利用技術群
    ├── /*/
    └── autoload.php
```
-->

### Setting file

- Setting file of EC-CUBE 3 is as below

#### Target file

1. /app/config/ec-cube/**config.yml**
-  Describe basic settings related to all the EC-CUBE 3 such as SSL communication and language.

1. /app/config/ec-cube/**database.yml**
- Describe settings related to Database connection such ass name of database or port.

1. /app/config/ec-cube/**mail.yml**
- Describe seetings related to e-mail option such as encryption allowing and authentication information.

1. /app/config/ec-cube/**path.yml**
- Setting the URL of such as Management・Front or path of upload file, ect...

### Constant

- Constant used in EC-CUBE 3 has been stored in the following


#### Target file

1. Common/**constants.php**
    - Constant of basic information of EC-CUBE version.

2. Resource/config/**constant.yml.dist**
    - Constant mainly used in the program.


### Quick replacement table for 2 and 3 systems
| 2 System                    | 3 System                                     |
|------------------------|------------------------------------------|
| SC_FormParam           | Eccube\Form\Type\                        |
| SC_Query               | Doctrine Orm                              |
| SC\_Helper\_Purchase     | Eccube\Service\PurchaseService           |
| LC\_Page\_Products\_Class | Eccube\Controller\ProductClassController |
| *.tpl                  | Eccube\Resouce\template\\*.twig                       |


## Reference

- <a href="http://sssslide.com/speakerdeck.com/amidaike/ec-cube3kodorideingu-number-1" target="_blank">EC-CUBE 3コードリーディング #1</a>
