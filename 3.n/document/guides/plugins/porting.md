# How to porting a Plugin 3.0.x to 3.n ?

## I. Apply new techniques
 - [Symfony 3.4](https://symfony.com/doc/3.4//index.html#gsc.tab=0)
 - [Annotation](https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/index.html)
 - [Injection](https://symfony.com/doc/3.4/components/dependency_injection.html)
 - [Doctrine ^2.6](https://symfony.com/doc/3.4/doctrine.html)

## II. Structure
<table>
<tr><th>3.0</th><th>3.n/4</th></tr>
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
- Extend AbstractController, many available variables are useful for development.
- By eliminating ServiceProvider, routing in the controller is easy to monitor. Using: @Route.
- You can injection multiple services/repositories by construct function.
- Many annotation you can use in controller: @Route, @Template, ...

<table>
<tr><th>3.0</th><th>3.n/4</th></tr>
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
// if you use @Template, then
return [
   'form' => $form->createView(),
];
---
// if you don't use it,
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

- Reference: https://github.com/EC-CUBE/sample-payment-plugin/blob/master/Controller/PaymentController.php

## IV. Entity
- By removed `Resource/doctrine/xxx.yml`, Entity will handle it by `annotation` (doctrine orm mapping)
- In new version, you can extend a table in core using `trait`.
- Support by inheritance mapping, you can define difference entity that uses the same table in core by `extends`.

1. Entity
<table>
<tr><th>3.0</th><th>3.n/4</th></tr>
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
- For form extension, you can use form theme for new attribute.
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
- Just change a little in construct function.
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
- By deleting doctrine directory, they change into entities (see IV Entity).
- By deleting migration directory, you can create data in PluginManager, please see below.
- In now, you can define new services/parameter/constant in config/services.yml
- locale/`message`.[ja,en].yml change to locale/`messages`.[ja,en].yml. And you can use locale/messages.[ja,en].php

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
<tr><th>3.0</th><th>3.n/4</th></tr>
<tr>
<td>

```
{% extends 'default_frame.twig' %}
```

</td>
<td>

```
{% extends '@admin/default_frame.twig' %}
```

</td>
</tr>
</table>

## VIII. ServiceProvider
- Removed on new version:
    - Routing will move to controller's annotation.
    - Service will move to Resouce/config/services.yml
    - Translator will move to Resouce/config/messages.[ja,en].yml
    - Navi of admin will move to Nav.php (see below)

## IX. Event
- By deleting event.yml, all hookpoint will defined in same [xxx]Event.php file.
- Remove some event: route, controller, ...
- New event: query customizer (see below)
- [xxx]Event.php with implements `Symfony\Component\EventDispatcher\EventSubscriberInterface`;
- In Template event, 2 new functions added. (addAsset and addSnippet)

<table>
<tr><th>3.0</th><th>3.n/4</th></tr>
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
- Remove some attributes:
    - Remove service provider
    - Service: declare in Resource/config/services.yml (see in Resource)
    - orm.path: removed migration so remove it.
    - const: move to Resource/config/services.yml (see in Resource)

<table>
<tr><th>3.0</th><th>3.n/4</th></tr>
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
- You can change some query builder

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

- Reference: http://doc3n.ec-cube.net/customize_repository

## XII. Nav
- You can add to navi of admin by create `Nav.php`

```php
class SamplePaymentNav implements \Eccube\Common\EccubeNav
{
    public static function getNav()
    {
        return [
            'order' => [
                'children' => [
                    'sample_payment_admin_payment_status' => [
                        'name' => 'sample_payment.admin.nav.payment_list',
                        'url' => 'sample_payment_admin_payment_status',
                    ]
                ]
            ],
        ];
    }
}
```

## XIII. Plugin manager
- You can migration data in here.

<table>
<thead>
<tr>
<th>3.0</th>
<th>3.n/4</th>
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
public function enable($config, ContainerInterface $container)
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

## XIV. Reference plugin
https://github.com/EC-CUBE/sample-payment-plugin
