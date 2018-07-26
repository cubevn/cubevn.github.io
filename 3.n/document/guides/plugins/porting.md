# How to porting a Plugin 3.0.x to 3.n ?

## I. Apply new techniques
 - [Annotation](https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/index.html)
 - [Injection](https://symfony.com/doc/3.4/components/dependency_injection.html)
 - [Doctrine 2.5](https://symfony.com/doc/3.4/doctrine.html)
 
## II. Config.xml

```yml
name: [Name of Plugin]
code: [Code of Plugin]
version: [Version of Plugin]
event: [Handle Class]                 # ★3.n: removed
service:                              # ★3.n: removed  
    - [Name of Plugin]ServiceProvider 
orm.path:                             # ★3.n: removed
    - /Resource/doctrine
const:                                # ★3.n: removed
    Constant_ABC: XYZ    
```

## III. PluginManager

```php
namespace Plugin\[Name of Plugin];
use Doctrine\ORM\EntityManager;
use Eccube\Plugin\AbstractPluginManager;
use Symfony\Component\DependencyInjection\ContainerInterface;

class PluginManager extends AbstractPluginManager
{
    /**
     * @param null $meta
     * @param Application|null $app
     * @param ContainerInterface $container
     * @throws \Doctrine\ORM\OptimisticLockException
     */
    public function enable($meta = null, Application $app = null, ContainerInterface $container)
    {
        /** @var EntityManager $em */
        $em = $container->get('doctrine.orm.entity_manager');
        $repo = $container->get(ProductRepository::class);
        $Entity = Product();
        ....
        $em->persist($Entity);
        $em->flush($Entity);
    }
}
```

## IV. Event

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Event                       
│       │      │     │    ├── [Name].php             
│       │      │     ├── [Name]Event.php             ☆3.0 : handle event 
                                                     ★3.n : will register listeners- handle for events
│       │      │     ├── event.yml                   ☆3.0 : will register listeners for events 
                                                     ★3.n : Removed
```
in 3.n , [Name]Event.php have format

```php
<?php
namespace Plugin\[Name];
use Eccube\Event\TemplateEvent;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Symfony\Component\HttpFoundation\RequestStack;
use Eccube\Repository\[Name]Repository;
use Eccube\Entity\[Name];

class [Name]Event implements EventSubscriberInterface
{
    /**
     * @var RequestStack
     */
    protected $requestStack;

    /**
     * EventSubscriber constructor.
     *
     * @param RequestStack $requestStack
     * @param TagRepository $tagRepository
     */
    public function __construct(
        RequestStack $requestStack
    ) {
        $this->requestStack = $requestStack;
    }
    /**
     * {@inheritdoc}
     *
     * @return array
     */
    public static function getSubscribedEvents()
    {
        return [
            '@admin/Product/index.twig' => ['onTemplateAdminProductIndex', 10],
            'Product/detail.twig' => ['onTemplateProductDetail', 10],
        ];
    }
    /**
     * Append js handle tagex update
     *
     * @param TemplateEvent $templateEvent
     */
    public function onTemplateAdminProductIndex(TemplateEvent $templateEvent)
    {
        $templateEvent->addSnippet('@[PluginCode]/admin/Product/xx_yy_zz.twig');
        $templateEvent->addSnippet('@[PluginCode]/admin/Product/xx_zz_kk.twig');
    }
    /**
     * Append js to customize tag display
     *
     * @param TemplateEvent $templateEvent
     */
    public function onTemplateProductDetail(TemplateEvent $templateEvent)
    {
        $templateEvent->addSnippet('@[PluginCode]/template_zz.twig');
    }
}
```

* Some events kind were defined in EC-CUBE[ will be confirm ]
    - System   : init & finish  
    - Route    : init  
    - Controler: request & response
    - Form     : form extension
    - Template : render
    - Specify  :


## V. ServiceProvider

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── ServiceProvider                       
│       │      │     │    ├── [plugin code]ServiceProvider.php       ☆3.0: Route & initialize components   
                                                                     ★3.n: removed ( instead : injection & annotation )    
```
## VI. Controller
```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Controller                       
│       │      │     │    ├──* 
```


```php
<?php
namespace Plugin\[PluginCode]\Controller;
use Eccube\Controller\AbstractController;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Template;
use Symfony\Component\HttpFoundation\Request;
/**
 * Controller to handle module setting screen
 */
class XxxxController extends AbstractController
{
    /**
     * @var YyyyRepository
     */
    private $yyyyRepository;
    /**
     * ConfigController constructor.
     * @param YyyyyRepository $yyyyRepository
     */
    public function __construct(YyyyRepository $yyyyRepository)
    {
        $this->yyyyRepository = $yyyyRepository;
    }
    /**
     * Edit config
     *
     * @param Request $request
     * @Route("/%eccube_admin_route%/plugin/[plugincode]/config", name="[route_name]")
     * @Template("@[Plugincode]/admin/config.twig")
     */
    public function edit(Request $request)
    {
        ....
    }
?>
```
## VI. Database
 - Schema
 
```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Resource                       
│       │      │     │    ├── doctrine                       
│       │      │     │    │    ├── Plugin.[plugin code].Entity.[Name].dcm.yml       ☆3.0: for install & delete
                                                                                ★3.n: removed   
```

 - Migration
 
```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Resource                       
│       │      │     │    ├── doctrine                       
│       │      │     │    │    ├── migration                       
│       │      │     │    │    │    ├── Version[date].php       ☆3.0: for install & update    
                                                                ★3.n: removed    
```

 - Entity

 ```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Entity                       
│       │      │     │      *    
```

 ```php
namespace Plugin\[Plugin code]\Entity;
use Doctrine\ORM\Mapping as ORM;
/**
 * Config
 *
 * @ORM\Table(name="[table name]")
 * @ORM\InheritanceType("SINGLE_TABLE")
 * @ORM\DiscriminatorColumn(name="discriminator_type", type="string", length=255)
 * @ORM\HasLifecycleCallbacks()
 * @ORM\Entity(repositoryClass="Plugin\[Plugin code]\Repository\[Table Name]Repository")
 */
class [Tbale Name] extends \Eccube\Entity\AbstractEntity
{
    const DEFAULT_NAME = 'condition';
    const AT_LEAST_ONE = 1;
    const MUST_ALL = 2;
    /**
     * @var string
     *
     * @ORM\Column(name="name", type="string", nullable=false, length=255)
     * @ORM\Id
     */
    private $name = self::DEFAULT_NAME;
    /**
     * @var string
     *
     * @ORM\Column(name="value", type="string", nullable=true, length=255)
     */
    private $value;
    
    public function setName($name)
    {
        $this->name = $name;
        
        return $this;
    }
    public function getName()
    {
        return $this->name;
    }
}
```
 - Repository
 ```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Repository                       
│       │      │     │      *    
```

 ```php
namespace Plugin\[Plugin Code]\Repository;
use Doctrine\ORM\EntityRepository;

class [TableName]Repository extends EntityRepository
{
}
 ```

## IX. Form : [PluginCode]/Form/

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Form                       
│       │      │     │      ├──Type
│       │      │     │      ├──Extension    
```

- Form Type
```php
namespace Plugin\[Plugin Name]\Form\Type;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Validator\Constraints as Assert;
class XxxxType extends AbstractType
{
	public function buildForm(FormBuilderInterface $builder, array $options)
	{
		$builder
			->add('xxxx', TextareaType::class, array(
				'required' => false,
				'constraints' => array(
					new Assert\NotBlank(),
				),
				'attr' => array(
					'id' => 'admin_yyy_zzz',
					'rows' => 25,
				),
			));
	}
	public function getName()
	{
		return 'asdfg_qwety';
	}
}
```
 - Form Extension

```php
namespace Plugin\[PluginCode]\Form\Extension\Admin;
use Eccube\Common\EccubeConfig;
use Eccube\Form\Type\Admin\ProductType;
use Symfony\Component\Form\AbstractTypeExtension;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Validator\Constraints as Assert;
/**
 * Class ProductTypeExtension
 */
class ProductTypeExtension extends AbstractTypeExtension
{
    /**
     * @var EccubeConfig
     */
    protected $eccubeConfig;
    /**
     * ProductTypeExtension constructor.
     *
     * @param EccubeConfig $eccubeConfig
     */
    public function __construct(EccubeConfig $eccubeConfig)
    {
        $this->eccubeConfig = $eccubeConfig;
    }
    /**
     * {@inheritdoc}
     */
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('author', TextType::class, [
                'label' => 'product_meta.admin.product.form.label.author',
                'required' => false,
                'constraints' => [
                    new Assert\Length([
                        'max' => $this->eccubeConfig['eccube_stext_len'],
                    ]),
                ],
            ])
            ->add('description', TextType::class, [
                'label' => 'product_meta.admin.product.form.label.description',
                'required' => false,
                'constraints' => [
                    new Assert\Length([
                        'max' => $this->eccubeConfig['product_meta.text.length'],
                    ]),
                ],
                'eccube_form_options' => [
                    'auto_render' => true,
                ],
            ])
        ;
    }
    /**
     * {@inheritdoc}
     */
    public function getExtendedType()
    {
        return ProductType::class;
    }
}
```

## IX. Template : [PluginCode]/Resource/template/index.yml
```php
{% extends '@admin/default_frame.twig' %}

{% set menus = ['setting'] %}

{% block title %}{{ 'note.admin.title'|trans }}{% endblock %}
{% block sub_title %}{{ 'note.admin.subtitle'|trans }}{% endblock %}

{% block main %}
    <!-- Bootstrap 4 -->
{% endblock %}
```

## IX. asset : [PluginCode]/Resource/assets/
```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── Resource                       
│       │      │     │    ├── Assets
│       │      │     │    │     ├── css
│       │      │     │    │     ├── js    
```

## VII. Navigation

```
EC-CUBE3n Root Directory
├──    app
│       ├── Plugin                                   
│       │      ├── [plugin code]                     
│       │      │     ├── ServiceProvider                       
│       │      │     │    ├── [plugin code]ServiceProvider.php       ☆3.0: Define menu in function {register}
                                                                     ★3.n: removed ( instead : Nav.php )    
```

## VIII. Translation: [PluginCode]/Resource/locale/messages.ja.yml
```yml
[pluginCode].[admin/front].[screen].[label]: 日本語。
```

## IX. Parametter : [PluginCode]/Resource/config/services.yml
```yml
parameters:
    note.data_file.path: '/../Resource/data/note.txt'
```


## IX. Technical Problems

1. Template : Confuse 
 - New way to adding or changing content in template: 

```php
   "short block" : like this '\{\{ eccube_block_hello({'name':'EC-CUBE'}) \}\}'.
```

 - Listen event of Admin template:
 
```php
    Admin/Product/product.twig  -> Admin/@admin/Product/product.twig
```

1. Event : 
 - Removed:
    - event: admin.order.delete.complete

1. Problems
    - Unable to retrieve the total order payment amount on the shopping index page.
    - The system still charges taxes on discounts.
    - The block.twig can not get the {http_cache} (esi) parameter.
    - Difficult to inject template (  edit inline, dont use id for element - xpath ).
    