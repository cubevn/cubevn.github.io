# プラグインを3.0.xから4.0に移植する 

## I. 新しく適用する技術
 - [Symfony 3.4](https://symfony.com/doc/3.4//index.html#gsc.tab=0)
 - [Annotation](https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/index.html)
 - [Injection](https://symfony.com/doc/3.4/components/dependency_injection.html)
 - [Doctrine ^2.6](https://symfony.com/doc/3.4/doctrine.html)

## II. 構成
<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>
<pre>
[plugin code]
    ├── Controller
    ├── Entity
    │     ├── [xxx].php
    │     ├── [none]
    │     ├── [none]
    ├── Form
    │     ├── Type
    │     ├── Extension
    ├── Repository
    ├── Resource
    │     ├── doctrine
    │     ├── migration
    │     ├── [none]
    │     ├── locale/message.[ja,en].yml
    │     ├── template
    ├── ServiceProvider
    ├── [Name]Event.php
    ├── config.yml
    ├── event.yml
    ├── [none]
    ├── [none]
    ├── PluginManager.php
</pre>
</td>
<td>
<pre>
[plugin code]
    ├── Controller
    ├── Entity
    │     ├── [xxx].php
    │     ├── [xxx]Trait.php        [new]
    │     ├── [xxx]Extend.php       [new]
    ├── Form
    │     ├── Type
    │     ├── Extension
    ├── Repository
    ├── Resource
    │     ├──                       [removed]
    │     ├──                       [removed]
    │     ├── config/service.yml    [new]
    │     ├── locale/messages.[ja,en].yml [change name]
    │     ├── template
    ├──                             [removed]
    ├── [Name]Event.php
    ├── config.yml
    ├──                             [removed]
    ├── [xxx]QueryCustomizer.php    [new]
    ├── Nav.php                     [new]
    ├── PluginManager.php
</pre>
</td>
</tr>
</table>

## III. Controller
- AbstractControllerを拡張すると、開発に役立つ変数が多く使用できます。
- ServiceProviderを削除することで、コントローラ内のルーティングが簡単に監視できます。 例：@Route。
- Construct機能で複数のサービス/リポジトリを注入することができます。
- Controllerで使用できるアノテーションが多くあります。例：@Method、@ Route、@Template、...

<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>
<pre lang="php">
public function index(Application $app, Request $request)
</pre>
</td>
<td>
<pre lang="php">
/**
 * @Method("POST")
 * @Route("/%eccube_admin_route%/plugin/ProductPriority/config", name="product_priority_admin_config")
 * @Template("@ProductPriority/admin/config.twig")
 */
public function index(Request $request)
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app['eccube.plugin.xxx']
</pre>
</td>
<td>
<pre lang="php">
public function __construct(xxxRepository $xxxRepository)
{
$this->xxxRepository = $xxxRepository;
}
use: $this->configRepository
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app['form.factory']
</pre>
</td>
<td>
<pre lang="php">
$this->formFactory (in AbstractController)
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app['eccube.plugin.xxx']
</pre>
</td>
<td>
<pre lang="php">
public function __construct(xxxRepository $xxxRepository)
{
$this->xxxRepository = $xxxRepository;
}
use: $this->xxxRepository
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app['orm.em']
</pre>
</td>
<td>
<pre lang="php">
$this->entityManager (in AbstractController)
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app->addSuccess
</pre>
</td>
<td>
<pre lang="php">
$this->addSuccess
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
return $app->render(
    'ProductPriority/Resource/template/admin/config.twig',
    [
        'form' => $form->createView(),
    ]
);
</pre>
</td>
<td>
<pre lang="php">
if you use @Template, then
return [
   'form' => $form->createView(),
];
---
if you don't use it,
return $this->render(
    '@ProductPriority/admin/config.twig',
    [
        'form' => $form->createView(),
    ]
);
</pre>
</td>
</tr>
<tr>
<td>
<pre lang="php">
$app->redirect($app->url('xxx'));
</pre>
</td>
<td>
<pre lang="php">
$this->redirectToRoute('xxx');
</pre>
</td>
</tr>
</table>

- 参考: https://github.com/EC-CUBE/sample-payment-plugin/blob/master/Controller/PaymentController.php

## IV. Entity
- `Resource/doctrine/xxx.yml`が削除され、その代わりに`annotation`を使用します。 (Doctrine orm)
-  新バージョンでは、 `trait`を使ってテーブルを拡張することができます。
- Inheritance mappingの適用として、 `extended`を使って、１つのテーブルに複数の異なるentityを定義することができます

1. Entity
<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>
<pre>
class Config extends AbstractEntity
</pre>
</td>
<td>
<pre>
use Doctrine\ORM\Mapping as ORM;
/**
 * @ORM\Table(name="plg_sample_payment_config")
 * @ORM\InheritanceType("SINGLE_TABLE")
 * @ORM\DiscriminatorColumn(name="discriminator_type", type="string", length=255)
 * @ORM\HasLifecycleCallbacks()
 * @ORM\Entity(repositoryClass="Plugin\SamplePayment\Repository\ConfigRepository")
 */
class Config extends AbstractEntity
</pre>
</td>
</tr>
<tr>
<td>
<pre>
private $id;
</pre>
</td>
<td>
<pre>
/**
 * @var int
 *
 * @ORM\Column(name="id", type="integer", options={"unsigned":true})
 * @ORM\Id
 * @ORM\GeneratedValue(strategy="IDENTITY")
 */
private $id;
</pre>
</td>
</tr>
</table>

2. Trait
```php
use Eccube\Annotation\EntityExtension;
use Doctrine\ORM\Mapping as ORM;
/**
 * @EntityExtension("Eccube\Entity\Customer")
 */
trait CustomerTrait
{
    /**
     * カードの記憶用カラム.
     *
     * @var string
     * @ORM\Column(type="smallint", nullable=true)
     */
    public $sample_payment_cards;
}
```

3. Extend
```php
use Eccube\Entity\Category;
use Doctrine\ORM\Mapping as ORM;
/**
 *
 * @ORM\Entity(repositoryClass="Plugin\ExtendCategory\Repository\ExtendCategoryRepository")
 */
class ExtendCategory extends Category
```

## V. Form
- `form_theme`というForm extensionの新しい属性が使用できます。

```php
class [ProductType]Extension extends AbstractTypeExtension
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder->add('description', TextType::class, [
            // ...
                'eccube_form_options' => [
                    'auto_render' => true,
                    'form_theme' => "@ExamplePlugin/admin/description.twig",
                ],
        ]);
    }
    // ...
}
```

## VI. Repository
- Construct機能が少し変更されます。
```php
use Eccube\Repository\AbstractRepository;
use Plugin\xxx\Entity\Config;
use Symfony\Bridge\Doctrine\RegistryInterface;

class ConfigRepository extends AbstractRepository
{
    public function __construct(RegistryInterface $registry)
    {
        // please pass the entity to construct
        parent::__construct($registry, Config::class);
    }
}
```

## VII. Resource
- Doctrineディレクトリが削除され, アノテーションでentityを対応します。 （IV. Entityを参考）
- Migrationディレクトリが削除され、PluginManagerでデータを生成することができます。（XIII. Plugin managerを参考）
- 現在、config/services.ymlで新しいservices/parameter/constantを定義することができます。
- locale/`message`.[ja,en].yml から locale/`messages`.[ja,en].ymlに変更します。locale/messages.[ja,en].phpも使用できます。

1. config/services.yml
```yml
# パラメータ定義
parameters:
   sample_payment.xxx: 1

# コンテナ定義
services:

```
2. template:

<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>
{% extends 'default_frame.twig' %}
</td>
<td>
{% extends '@admin/default_frame.twig' %}
</td>
</tr>
</table>

## VIII. ServiceProvider
- 新バージョンでは
    - Routingはコントローラのアノテーションで対応します。
    - ServiceはResouce / config / services.ymlで対応します。
    - TranslatorはResouce / config / messagesで対応します。[ja、en] .yml
    - 管理画面のメニュー（Navi）はNav.phpで対応します。（XII. Navをご参照）

## IX. Event
- event.ymlが削除され、全てのhookpointは同じ[xxx]Event.php fileで定義することになります。
- いくつかのeventが削除されます：route、controllerなど
- 新しいevent: query customizer (XI. QueryCustomizerをご参考)
- [xxx]Eventクラスでは`Symfony\Component\EventDispatcher\EventSubscriberInterface`が必須になります。
- 新しいEventのテンプレートでは、機能が2つ追加します（addAssetとaddSnippet）

<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>
// event.yml

```yml
admin.product.edit.complete:
    - [onAdminProductEditComplete, NORMAL]
Admin/Order/edit.twig:
    - [onAdminOrderEditTwig, NORMAL]
```
</td>
<td>
// [xxx]Event.php

```php
/**
 *例：
 * - array('eventName' => 'methodName')
 * - array('eventName' => array('methodName', $priority))
 * - array('eventName' => array(array('methodName1', $priority), array('methodName2')))
 */
public static function getSubscribedEvents()
{
    return [
        EccubeEvents::ADMIN_PRODUCT_EDIT_COMPLETE => 'onAdminProductEditComplete'
        '@admin/Order/edit.twig' => 'onAdminOrderEditTwig',
    ];
}
```
</td>
</tr>
<tr>
<td>
[xxx]Event.php

```php
public function onProductDetailRender(TemplateEvent $event)
{
...
$twig = $this->app['twig'];
$twigAppend = $twig->getLoader()->getSource('[plugin code]/Resource/template/default/xxx.twig');
$twigSource = $event->getSource();
// Render new template by `str_replace`
$twigSource = $this->renderPosition($twigSource, $twigAppend, $this->pluginTag);
...
}
```
</td>
<td>
[xxx]Event.php

```php
public function onProductDetailRender(TemplateEvent $event)
{
    // position will define by javascript in product_review.twig file
    $event->addSnippet('@[plugin code]/default/xxx.twig');
}
```

[plugin code]/Resource/template/default/xxx.twig

```js
$(function () {
    $('#new_area').appendTo($('div.ec-layoutRole__main'));
});
```

```html
<div id="new_area" class="ec-role">
...
</div>
```
</td>
</tr>
</table>

## X. config.yml
- 削除されたもの：
    - Service provider
	- orm.path
- 別の場所に配置されたもの（VII. Resourceをご参考）：
	- Service:Resource/config/services.yml
    - const: Resource/config/services.yml

<table>
<tr><th>3.0</th><th>4.0</th></tr>
<tr>
<td>

```yml
name: [Name of Plugin]
code: [Code of Plugin]
version: [Version of Plugin]
event: [Handle Class]
service:
    - [Name of Plugin]ServiceProvider
orm.path:
    - /Resource/doctrine
const:
    Constant_ABC: XYZ
```
</td>
<td>

```yml
name: [Name of Plugin]
code: [Code of Plugin]
version: [Version of Plugin]
```
</td>
</tr>
</table>

## XI. QueryCustomizer
- いくつかのクエリビルダーを変更することができます。

<table>
  <thead>
    <tr>
      <th>Repository class</th>
      <th>QueryKey</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ProductRepository::getQueryBuilderBySearchData()</td>
      <td>QueryKey::PRODUCT_SEARCH</td>
    </tr>
    <tr>
      <td>ProductRepository::getQueryBuilderBySearchDataForAdmin()</td>
      <td>QueryKey::PRODUCT_SEARCH_ADMIN</td>
    </tr>
    <tr>
      <td>ProductRepository::getFavoriteProductQueryBuilderByCustomer</td>
      <td>QueryKey::PRODUCT_GET_FAVORITE</td>
    </tr>
    <tr>
      <td>CustomerRepository::getQueryBuilderBySearchData()</td>
      <td>QueryKey::CUSTOMER_SEARCH</td>
    </tr>
    <tr>
      <td>OrderRepository::getQueryBuilderBySearchData()</td>
      <td>QueryKey::ORDER_SEARCH</td>
    </tr>
    <tr>
      <td>OrderRepository.getQueryBuilderBySearchDataForAdmin()</td>
      <td>QueryKey::ORDER_SEARCH_ADMIN</td>
    </tr>
    <tr>
      <td>OrderRepository::getQueryBuilderByCustomer()</td>
      <td>QueryKey::ORDER_SEARCH_BY_CUSTOMER</td>
    </tr>
  </tbody>
</table>

```php
class AdminProductListCustomizer extends OrderByCustomizer
{
    protected function createStatements($params, $queryKey)
    {
        return [new OrderByClause('p.id')];
    }

    public function getQueryKey()
    {
        return QueryKey::PRODUCT_SEARCH_ADMIN;
    }
}
```

- 参考: http://doc3n.ec-cube.net/customize_repository

## XII. Nav
- Navi（管理画面のメニュー）を追加する時、`Nav.php`を作成することになります。

```php
class SamplePaymentNav implements \Eccube\Common\EccubeNav
{
    public static function getNav()
    {
        return [
            'order' => [
                'id' => 'sample_payment_admin_payment_status',
                'name' => 'sample_payment.admin.nav.payment_list',
                'url' => 'sample_payment_admin_payment_status',
            ],
        ];
    }
}
```

## XIII. Plugin manager
- こちらでデータを移行することができます。

<table>
<thead>
<tr>
<th>3.0</th>
<th>4.0</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<pre>
public function enable($config, $app)
</pre>
</td>
<td>
<pre>
public function enable($config = [], $app = null, ContainerInterface $container)
</pre>
</td>
</tr>
<tr>
<td>
<pre>
$entityManager = $app['orm.em'];
</pre>
</td>
<td>
<pre>
$entityManager = $container->get('doctrine.orm.entity_manager');
</pre>
</td>
</tr>
<tr>
<td>
<pre>
// repository/service
$app['eccube.plugin.xxxx'];
</pre>
</td>
<td>
<pre>
$container->get([Repository,Service]::class);
</pre>
</td>
</tr>
</tbody>
</table>

## XIV. 参考プラグイン
https://github.com/EC-CUBE/sample-payment-plugin
