---
layout: default
title: Phát triển plugin cho version 3.0.15
---

---

# Plugin specification

Specifications related to plugin can be downloaded from the following.

- <a href="http://downloads.ec-cube.net/src/manual/v3/plugin.pdf" target="_blank">ECCUBE 3プラグイン仕様</a>

# Tutorial

Please refer the following for Sample Plugin and Plugin's tutorial.

- <a href="https://github.com/EC-CUBE/category-content-plugin" target="_blank">カテゴリコンテンツ(サンプルプラグイン)</a>

Making plugin tutorial

- <a href="http://qiita.com/chihiro-adachi/items/6318642120f67faedf0b" target="_blank">EC-CUBEのプラグインを作る(3.0.9向け)</a>

# Publishing to Owner store

Please refer the following for detail about how to publish to EC-CUBE's Owner store.

- <a href="http://www.ec-cube.net/plugin/" target="_blank">EC-CUBEプラグインをつくろう！</a>


# Phát triển plugin cho version 3.0.15
Trước phiên bản 3.0.15 thì sẽ làm theo các bước sau khi muốn tạo một plugin.

1. Tạo những file bắt buộc
2. Nén thư mục
3. Install từ màn hình admin
4. Enable plugin, sủ dụng
Tại phiên bản 3.0.15 thì để tăng hiệu năng phát triển thì thêm vào các tính năng sau đây
1. Hiển thị đanh sách plugin chưa được cài đặt
2. Thao tác với plugin bằng câu lệnh console(cài đăt, enable, disable)

## **Cấu trúc thư mục**
EC-CUBE3 Root Directory > app > Plugin

```
EC-CUBE3 Root Directory
├──    app
│       ├── cache
│       ├── config
│       ├── console
│       ├── log
│       ├── Plugin ★Plugins Folder
│       │      ├── CategoryContent  ☆Thư mục chứa plugin của tutorial này
```
Vì lẽ đó, sẽ tạo cấu trúc thư mục giống như trên, tiến hành viết code trên thư mục đó.

## Bắt đầu tạo plugin thôi!!!
Lần này sẽ tạo một plugin tên là CategoryContext.Cái này đơn giản chỉ là thêm một form nhập liệu để giải thích về
category mà thôi.

```
Admin Page : Product>Add Category, Thêm một textarea để nhập description về category đó
Front Page : Trang frontend , click vào category đã tạo , đơn giản hiện ra nội dung đã nhập ở phần admin thôi.
Dễ như ăn kẹo ấy mà !
```

## Luồng tạo plugin
Tạo theo các bước như dưới đây。

### Chuẩn bị cài đặt plugin( Tạo folder )

#### Thiết lập cơ bản
1. Tạo thư mục chứa plugin
1. Tạo file **config.yml**
1. Tạo các file khác(để sau)

#### Tạo table và định nghĩa bảng
1. Tạo migration file
1. Tạo migration manager
1. Tạo bảng (Thực thi file migration)
   - ※Xác nhận DB
1. Tạo Entity
1. Tạo file dcm.yml

### Xây dựng Plugin

#### Tạo event
1. Chỉnh sửa file **config.yml**
1. Tạo file **event.yml**
1. Tạo event class
   - ※Xác nhận hoạt động

#### Tạo Form
1. Thêm mục vào trong Form
1. Sửa file **event.yml**
1. Chỉnh sửa file event class
1. Tạo ServiceProvider
1. Sửa file **config.yml**
1. Lưu dữ liệu
   - ※Xác nhận hoạt động


## Cài đặt plugin
Trước tiên tạo thư mục theo cấu trúc sau `app/Plugin/CategoryContext`

### Tạo những file cài đặt cần thiết

Tao file `config.yml`

```yaml:config.yml
name: CategoryContent
code: CategoryContent
version: 1.0.0
```

### Xác nhận hiển thị
Sau khi tao xong folder và file `config.xml` thì vào
`Admin > Owner Store > Plugin > Plugin List` sẽ thấy plugin vừa tạo hiển thị ở phần plugin chưa được cài đặt

### Tạo các file khác
Do lần này là tutorial nên chúng ta tạo các file như dưới đây và thao tác.

`event.yml : Định nghĩa event `

```yaml:event.yml
De trong
```

`PluginManager.php : Quản lý plugin (enable, disable, install , uninstall)`

```php:PluginManager.php

namespace Plugin\CategoryContent;

use Eccube\Plugin\AbstractPluginManager;

class PluginManager extends AbstractPluginManager
{

    public function install($config, $app)
    {
    }

    public function uninstall($config, $app)
    {
    }

    public function enable($config, $app)
    {
    }

    public function disable($config, $app)
    {
    }

    public function update($config, $app)
    {
    }
}
```
Tao folder/file theo cấu trúc như sau `ServiceProvider/CategoryContentServiceProvider.php`

```php:ServiceProvider/CategoryContentServiceProvider.php
<?php
namespace Plugin\CategoryContent\ServiceProvider;

use Eccube\Application;
use Silex\Application as BaseApplication;
use Silex\ServiceProviderInterface;

class CategoryContentServiceProvider implements ServiceProviderInterface
{
    public function register(BaseApplication $app)
    {
    }

    public function boot(BaseApplication $app)
    {
    }
}
```
`Resource/doctrine/Plugin.CategoryContent.Entity.CategoryContent.dcm.yml`

```yaml:Resource/doctrine/Plugin.CategoryContent.Entity.CategoryContent.dcm.yml
De trong
```
Tao file giao diện `Resource/template/Admin/category.twig`

```twig:Resource/template/Admin/category.twig
De trong
```

Thao tác DB `Repository/CategoryContentRepository.php`

```php:Repository/CategoryContentRepository.php

namespace Plugin\CategoryContent\Repository;

use Doctrine\ORM\EntityRepository;

class CategoryContentRepository extends EntityRepository
{
}
```

Migration `Migration/Version20160218160500.php`. Đặt tên file theo dạng Version[yyyymmddHHiiss].php

```php:Migration/Version20160218160500.php
<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

class Version20160218160500 extends AbstractMigration
{
    public function up(Schema $schema)
    {
    }

    public function down(Schema $schema)
    {
    }
}
```

Tạo Form `Form/Extension/CategoryContentExtension.php`

```php:Form/Extension/CategoryContentExtension.php

namespace Plugin\CategoryContent\Form\Extension;

use Symfony\Component\Form\AbstractTypeExtension;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\FormInterface;
use Symfony\Component\Form\FormView;

class CategoryContentExtension extends AbstractTypeExtension
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
    }

    public function buildView(FormView $view, FormInterface $form, array $options)
    {
    }

    public function getExtendedType()
    {
    }
}
```

Tạo Entity `Entity/CategoryContent.php`


```php:Entity/CategoryContent.php
<?php

namespace Plugin\CategoryContent\Entity;

class CategoryContent extends \Eccube\Entity\AbstractEntity
{
}
```

File xử lý event `CategoryContentEvent.php`

```php:CategoryContentEvent.php
De trong
```


Sau đó ta sẽ được thư mục có cấu trúc như dưới đây.

```
Cấu trúc thư mục của plugin
├── CategoryContentEvent.php
├── config.yml
├── Entity
│      └── CategoryContent.php
├── event.yml
├── LICENSE.txt
├── Migration
│      └── Version20160218160500.php
├── PluginManager.php
├── Repository
│      └── CategoryContentRepository.php
├── Resource
│       ├── doctrine
│       │      └── Plugin.CategoryContent.Entity.CategoryContent.dcm.yml
└── ServiceProvider
        └── CategoryContentServiceProvider.php
```


### Định nghĩa bảng
Mục đích của plugin lần này là lưu description category nên chúng ta định nghĩa một bảng để lưu trữ nó nào.
Khi tạo DB chúng ta sử dụng tính năng tên là migration được định nghĩa như dưới đây.

```php:Migration/Version20160218160500.php
<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

class Version20160218160500 extends AbstractMigration
{
    public function up(Schema $schema)
    {
        $this->createPluginTable($schema);
    }

    public function down(Schema $schema)
    {
        $schema->dropTable('plg_category_content');
    }

    protected function createPluginTable(Schema $schema)
    {
        $table = $schema->createTable("plg_category_content");
        $table->addColumn('category_id', 'integer');
        $table->addColumn('content', 'text', array('notnull' => false));
        $table->setPrimaryKey(array('category_id'));
    }
}
```

### Tạo PluginManager.php
Sau khi tạo xong file migration , để tạo bảng thì chúng ta định nghĩa file `PluginManager.php`
```php:PluginManager.php
<?php
namespace Plugin\CategoryContent;

use Eccube\Plugin\AbstractPluginManager;

class PluginManager extends AbstractPluginManager
{
    // インストール時にマイグレーションの「up」メソッドを実行します
    public function install($config, $app)
    {
        $this->migrationSchema($app, __DIR__.'/Migration', $config['code']);
    }

    // アンインストール時にマイグレーションの「down」メソッドを実行します
    public function uninstall($config, $app)
    {
        $this->migrationSchema($app, __DIR__.'/Migration', $config['code'], 0);
    }

    // プラグイン有効時に、指定の処理 (ファイルのコピーなど) を実行できます。(今回はなし)
    public function enable($config, $app)
    {

    }

    // プラグイン無効時に、指定の処理 ( ファイルの削除など ) を実行できます。(今回はなし)
    public function disable($config, $app)
    {

    }

    // プラグインアップデート時に、指定の処理を実行できます。(今回はなし)
    public function update($config, $app)
    {

    }
}
```

### Tạo bảng DB
Khởi động console, chúng ta sẽ cài đặt plugin bằng command. Khi này sẽ thông qua file `PluginManager.php`

```shell-session
$cd EC-CUBE3 root directory
$php app/console plugin:develop install --code CategoryContent
```

### Kiểm tra bảng vừa tạo
Truy cập vào DB kiểm tra xem bảng `「plg_category_content」`đã được tao chưa

### Define Entity
Mỗi khi update, add nội dung form thì nó sẽ phản ánh trong DB. Thông tin của table được biểu hiện qua entity
`Entity/CategoryContent.php`

```php:Entity/CategoryContent.php
<?php
namespace Plugin\CategoryContent\Entity;

class CategoryContent extends \Eccube\Entity\AbstractEntity
{
    private $id;

    private $content;

    public function getId()
    {
        return $this->id;
    }

    public function setId($id)
    {
        $this->id = $id;

        return $this;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function setContent($content)
    {
        $this->content = $content;

        return $this;
    }
}
```

### Định nghĩa dcm.yml
Dữ liệu được lưu trữ trong Entity , để có thể lưu xuống DB chúng ta sử dụng file dcm.yml(thực chất đây chính là file
định nghĩa DB table và map giữa DB vs Entity)

```yaml:Resource/doctrine/Plugin.CategoryContent.Entity.CategoryContent.dcm.yml
Plugin\CategoryContent\Entity\CategoryContent:
    type: entity
    table: plg_category_content
    repositoryClass: Plugin\CategoryContent\Repository\CategoryContentRepository
    id:
        id:
            type: integer
            nullable: false
            unsigned: false
            id: true
            column: category_id
            generator:
                strategy: NONE
    fields:
        content:
            type: text
            nullable: true
    lifecycleCallbacks: {  }
```


## Cấu trúc Plugin

### Tạo event
`Admin > Product > Category` truy cập vào link trên thì sẽ hiện ra màn hình nhập Category, Trong đó nhiệm vụ của plugin
này là thêm một textarea vào để viết description cho category. Để thêm được chúng ta sẽ sử dụng form và thêm một item
vào. Từ phiên bản 3.0.9 trở đi thì sử dụng HookPoints để bắt sự kiện. Nó được định nghĩa như dưới đây.

    - config.yml : Định nghĩa file xử lý event
    - event.yml  : Định nghĩa event
    - Chúng ta sẽ tạo thêm file mới dùng để xử lý event.

### config.yml
Định nghĩa file sử lý event trong `config.yml`

```yaml:config.yml
name: CategoryContent
code: CategoryContent
version: 1.0.0
event: CategoryContentEvent ★Class handle event
```

### event.yml
Event mà sử lý việc save category ở màn hình admin thì được định nghĩa ở hookpoint dưới đây

    - admin.product.category.index.initialize
    - admin.product.category.index.complete

Vì lần này thêm item vào form nên sử dụng event initialize
event.yml sẽ được sửa lại như sau.

```yaml:event.yml
admin.product.category.index.initialize: ★CategoryContent hook point
    - [onAdminProductCategoryInit, NORMAL]  ★Method Name
```

Note.) Ở phần method name thì đầu tiên là 4 khoảng trắng, sau đó đến đấu - rồi tiếp theo một khoảng trắng rồi đến
tên method. Nếu mà không định nghĩa chuẩn thì hệ thống sẽ không nhận nên chú ý điều đó. Tốt nhất nên copy code trước
cho yên tâm .

※Tham khảo danh sách hook point tại [Plugin Design(Japanese)](http://downloads.ec-cube.net/src/manual/v3/plugin.pdf)

### Tạo file xử lý event
Cho đến bây giờ chúng ta đã làm được

    - Khi gọi đến hook point của category controller
    - chạy vào method onFormInitializeAdminProductCategory của class CategoryContentEvent

File `CategoryContentEvent`

```php:CategoryContentEvent.php
<?php
namespace Plugin\CategoryContent;

use Eccube\Event\EventArgs;

class CategoryContentEvent
{
    /** @var \Eccube\Application $app */
    private $app;

    public function __construct($app)
    {
        $this->app = $app;
    }

    public function onFormInitializeAdminProductCategory(EventArgs $event)
    {
        dump(111);
    }
```

Trước tiên chúng ta tạo một method trống để kiểm tra xem bắt được event hay chưa.
Truy cập vào `Admin > Product > Add Cateogry` click nút `Create Category` nếu hiện ra `111` có nghĩa là
 đã bắt được sự kiện


## Tạo form

### Định nghĩa các mục mở rộng
Chúng ta định nghĩa item sẽ thêm vào form lưu dữ liệu nhé.

File : `CategoryContentEvent.php`

```php:CategoryContentEvent.php
<?php
namespace Plugin\CategoryContent;

use Eccube\Application;
use Eccube\Common\Constant;
use Eccube\Entity\Category;
use Eccube\Event\EventArgs;
use Eccube\Event\TemplateEvent;
use Plugin\CategoryContent\Entity\CategoryContent;
use Symfony\Component\Form\FormInterface;
use Symfony\Component\HttpKernel\Event\FilterResponseEvent;

class CategoryContentEvent
{
    /**
     * プラグインが追加するフォーム名
     */
    const CATEGORY_CONTENT_TEXTAREA_NAME = 'plg_category_content';

    /** @var \Eccube\Application $app */
    private $app;

    public function __construct($app)
    {
        $this->app = $app;
    }

    public function onFormInitializeAdminProductCategory(EventArgs $event)
    {
        /** @var Category $target_category */
        $TargetCategory = $event->getArgument('TargetCategory');
        $id = $TargetCategory->getId();

        $CategoryContent = null;

        // IDの有無で登録か編集かを判断
        if ($id) {
            // カテゴリ編集時は初期値を取得
            $CategoryContent = $this->app['category_content.repository.category_content']->find($id);
        }

         // カテゴリ新規登録またはコンテンツが未登録の場合
        if (is_null($CategoryContent)) {
            $CategoryContent = new CategoryContent();
        }

        // フォームの追加
        /** @var FormInterface $builder */
        // FormBuildeの取得
        $builder = $event->getArgument('builder');
        // 項目の追加
        $builder->add(
            self::CATEGORY_CONTENT_TEXTAREA_NAME,
            'textarea',
            array(
                'required' => false,
                'label' => false,
                'mapped' => false,
                'attr' => array(
                    'placeholder' => 'コンテンツを入力してください(HTMLタグ使用可)',
                ),
            )
        );

        // 初期値を設定
        $builder->get(self::CATEGORY_CONTENT_TEXTAREA_NAME)->setData($CategoryContent->getContent());

    }
}
```

Sau khi tạo xong vào màn hình add category sẽ thấy xuất hiện một form add dữ liệu
注.) Nhất định là phải chỉ định **'mapped' => false** nếu không sẽ xuất hiện lỗi.


### event.yml
Để lưu được form ở trên, thì chúng ta sẽ dùng nốt hook point còn lại `admin.product.category.index.complete` . Sửa lại
file `event.yml` như sau.

```yaml:event.yml
admin.product.category.index.initialize: ★Category controller hook point
    - [onAdminProductCategoryInit, NORMAL]  ★Method name
admin.product.category.index.complete:   ☆Hook point dành cho save form
    - [onAdminProductCategoryEditComplete, NORMAL]   ☆Method name
```

### CategoryContentEvent.php
Thêm phương thức xử lý submit form category

```php:CategoryContentEvent.php
    /**
     * 管理画面：カテゴリ登録画面で、登録処理を行う.
     *
     * @param EventArgs $event
     */
    public function onAdminProductCategoryEditComplete(EventArgs $event)
    {
        /** @var Application $app */
        $app = $this->app;
        /** @var Category $target_category */
        $TargetCategory = $event->getArgument('TargetCategory');
        /** @var FormInterface $form */
        $form = $event->getArgument('form');

        // 現在のエンティティを取得
        $id = $TargetCategory->getId();
        // フォーム値のIDをもとに登録カテゴリ情報を取得
        $CategoryContent = $app['category_content.repository.category_content']->find($id);
        if (is_null($CategoryContent)) {
            $CategoryContent = new CategoryContent();
        }

        // エンティティを更新
        $CategoryContent
            ->setId($id)
            ->setContent($form[self::CATEGORY_CONTENT_TEXTAREA_NAME]->getData());

        // DB更新
        $app['orm.em']->persist($CategoryContent);
        $app['orm.em']->flush($CategoryContent);
    }
```

### Tạo ServiceProvider
Là lớp định nghĩa các biền toàn cục như  các Repository và các Form

```php:ServiceProvider/CategoryContentServiceProvider.php
<?php
namespace Plugin\CategoryContent\ServiceProvider;

use Eccube\Application;
use Silex\Application as BaseApplication;
use Silex\ServiceProviderInterface;

class CategoryContentServiceProvider implements ServiceProviderInterface
{
    public function register(BaseApplication $app)
    {
        //Repository
        $app['category_content.repository.category_content'] = $app->share(function () use ($app) {
            return $app['orm.em']->getRepository('Plugin\CategoryContent\Entity\CategoryContent');
        });

    }

    public function boot(BaseApplication $app)
    {
    }
}
```

### Edit file config.yml
Sau khi đã hoàn tất việc chỉnh sửa file ServiceProvider thì update lại nội dung file config.yml

```yaml:config.yml
service:
    - CategoryContentServiceProvider
orm.path:
    - /Resource/doctrine
```

### Xác nhận kết quả
Sau khi đã hoàn thành các bước trên chúng ta kiểm tra lại bằng cách thêm một category mới.
`Admin > Product > Category` nhập tên và miêu tả của category nhấn nút submit .
Nếu màn hình hiện ra dòng chữ **「カテゴリを保存しました」** thì có nghĩa đã submit thành công.
Và code cũng đã hoàn thành.

## Kiểm tra trên Frotent
Để có thể hiển thị thông tin category đã đăng ký trên Frontent thì chúng ta cần xử lý phần View.

### Chỉnh sửa file `event.yml` thêm phần xử lý twig event.yml

```yaml:event.yml
# front page event
Product/list.twig:
    - [onRenderProductList, NORMAL]
```

### Chỉnh sửa class handle event( thên xử lý hiển thị front )

Thêm method onRenderProductList vào file `CategoryContentEvent.php`

```php:CategoryContentEvent.php
    /**
     * 商品一覧画面にカテゴリコンテンツを表示する.
     *
     * @param TemplateEvent $event
     */
    public function onRenderProductList(TemplateEvent $event)
    {
        $parameters = $event->getParameters();

        // カテゴリIDがない場合、レンダリングしない
        if (is_null($parameters['Category'])) {
            return;
        }

        // 登録がない、もしくは空で登録されている場合、レンダリングをしない
        $Category = $parameters['Category'];
        $CategoryContent = $this->app['category_content.repository.category_content']
            ->find($Category->getId());
        if (is_null($CategoryContent) || $CategoryContent->getContent() == '') {
            return;
        }

        // twigコードにカテゴリコンテンツを挿入
        $snipet = '<div class="row">{{ CategoryContent.content | raw }}</div>';
        $search = '<div id="result_info_box"';
        $replace = $snipet.$search;
        $source = str_replace($search, $replace, $event->getSource());
        $event->setSource($source);

        // twigパラメータにカテゴリコンテンツを追加
        $parameters['CategoryContent'] = $CategoryContent;
        $event->setParameters($parameters);
    }
```

### Kiểm tra hiển thị
Click vào Category đã tạo ở bên admin, nếu thấy hiển thị ra category name vs description là thành công.
Chúc bạn may mắn!!!
