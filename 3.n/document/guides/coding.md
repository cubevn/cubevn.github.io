---
layout: default
title: Code standard
---

---

## Coding Style in EC-CUBE

### Rules

* URL：not append / at the end of URL
  + `https://example.com`
  + `https://example.com/product`
  + `https://example.com/product/new`
  + `https://example.com/product/2/edit`

    - Trong trường hợp new thì không cần truyền tham số
    - Trong trường hợp edit thì nếu không truyền tham số sẽ phát sinh Exception(BadRequest)
    - Trong trường hợp edit thì dù truyền tham số mà tham số đó không tồn tại sẽ phát sinh Exception(NotFound)

* FileName, FolderName
  + Follow of UpperCamel :
    - `ControllerClassName.php`
    - `FormNameType.php`
    - `EntityName.php` `Master/EntityName.php`
  + Except these special folders:  data, app

* Template name (seperation with snack _ )
  + `TemplateDir/template_name.twig`

* Code PHP
  + Base on [Symfony code standard](http://symfony.com/doc/current/contributing/code/standards.html)
  + Code Standard (as demo code)

```
<?php

/*
 * This file is part of the Symfony package.
 *
 * (c) Fabien Potencier <fabien@symfony.com>
 *
 * For the full copyright and license information, please view the LICENSE
 * file that was distributed with this source code.
 */

namespace Acme;

/**
 * Coding standards demonstration.
 */
class FooBar
{
    const SOME_CONST = 42;

    /**
     * @var string
     */
    private $fooBar;

    /**
     * @param string $dummy Some argument description
     */
    public function __construct($dummy)
    {
        $this->fooBar = $this->transformText($dummy);
    }

    /**
     * @return string
     *
     * @deprecated
     */
    public function someDeprecatedMethod()
    {
        @trigger_error(sprintf('The %s() method is deprecated since version 2.8 and will be removed in 3.0. Use Acme\Baz::someMethod() instead.', __METHOD__), E_USER_DEPRECATED);

        return Baz::someMethod();
    }

    /**
     * Transforms the input given as first argument.
     *
     * @param bool|string $dummy   Some argument description
     * @param array       $options An options collection to be used within the transformation
     *
     * @return string|null The transformed input
     *
     * @throws \RuntimeException When an invalid option is provided
     */
    private function transformText($dummy, array $options = array())
    {
        $defaultOptions = array(
            'some_default' => 'values',
            'another_default' => 'more values',
        );

        foreach ($options as $option) {
            if (!in_array($option, $defaultOptions)) {
                throw new \RuntimeException(sprintf('Unrecognized option "%s"', $option));
            }
        }

        $mergedOptions = array_merge(
            $defaultOptions,
            $options
        );

        if (true === $dummy) {
            return;
        }

        if ('string' === $dummy) {
            if ('values' === $mergedOptions['some_default']) {
                return substr($dummy, 0, 5);
            }

            return ucwords($dummy);
        }
    }

    /**
     * Performs some basic check for a given value.
     *
     * @param mixed $value     Some value to check against
     * @param bool  $theSwitch Some switch to control the method's flow
     *
     * @return bool|null The resultant check if $theSwitch isn't false, null otherwise
     */
    private function reverseBoolean($value = null, $theSwitch = false)
    {
        if (!$theSwitch) {
            return;
        }

        return !$value;
    }
}
```

  +  personalized of EC-CUBE
    + Variable in `entity` using UpperCamel
    + parammetters in  render twig :
       - Entity : UpperCamel
       - Other variables :  Snack with _
  + Code standard

```php

// HogeController

class HogeController
{
    protected $title;

    protected $subTitle;

    public function index(Application $app, $id)
    {
        $Product = $app['eccube.repository.product']->find($id);

        $totalCount = 1;

        // ...

        $app->render('path/to/hoge.twig', array(
            'form' => $form->createView(),
            'Product' => $Product,
            'total_count' => $totalCount,
        ))
    }
}

// hoge.twig

{{ total_count }}

{{ Product.name }}
{{ Product.free_area }}

{% for ProductClass in Product.ProductClasses %}
    {{ ProductClass.price01 }}
    {{ ProductClass.price02 }}
{% endfor %}

{% for OrderDetailForm in form.OrderDetails %}
    {{ form_widget(OrderDetailForm.product_name) }}
    {{ form_widget(OrderDetailForm.price) }}
    {{ form_widget(OrderDetailForm.quantity) }}
{% endfor %}
```

* Database
  + Theo như https://github.com/EC-CUBE/ec-cube/issues/210
    - Table name using snack _
    - prefix dtb_
    - prefix mtb_
    - Flag is smallint

* Position of semi-colon

```php
$builder
　　->add('name')
　　->add('age'); // here
```

```php
$array = array(
　　'name' => 'shinichi',
　　'age' => '26',
); // here
```

* Using \ with namespace
    + Using in
        - muốn dùng những namespace không được định nghĩa bởi câu lệnh use
        - PHP standard namespace : `new \Datetime()`
    + Not use in
        - `use Namespace`
        - With entity `Eccube\Entity\Xxx`
          ex: `$app['orm.em']->getRepository('Eccube\Entity\Xxx');`


* Vị trí hiển thị của `form_errors`
  * Hiển thị ở dưới ô nhập liệu tương ứng

```
    form_widget(form.name)
    form_errors(form.name)
```

* FlashMessage
  + Sử dụng khi thay đổi namespace tại các màn hình front hay admin
  + Có 4 loai level như dưới đây
      - `$app->addSuccess('It works!');` // front.success
      - `$app->addInfo('You got a new message!');` // front.info
      - `$app->addWarning('Check your mail address');` // front.warning
      - `$app->addDanger('Error occured!!', 'admin');` // admin.danger
      - `$app->addError('Can't delete this section', 'admin');` // admin.error

## Giải thích về các thành phần chính của EC-CUBE

Code EC-CUBE thì toàn bộ nằm trong thư mục `src/Eccube`

| Where | What | in 2.13 version　 |
|------|------|------|
| ControllerProvider | Định nghĩa Routing. Truyền request đến controller | html/Các trang php ở dưới thư mục này |
| Controller | Nhận request, xử lý và trả lại response cho view | LC_Page_Xxx |
| Service | Tầng Business logic. Đảm nhận những xử lý logic | SC_Xxx |
| ServiceProvider | Đăng ký những biến toàn cục của hệ thông  | SC_Initial hoặc là Require_base |
| Resource/doctrine | Nơi đặt file định nghĩa của doctrine. Viết bằng yaml | Không có |
| Entity | Thực thể để map với các doctrine tương ứng| Không có |
| Repository | Định nghĩa các hàm tương tác với bảng dữ liệu trong Entity | SC_Query |
| Form/Type | Định nghĩa Form, Validator... | SC_FormParam |
| View | View viết bằng Twig | Xxx.tpl |


## Để Controller không qúa phức tạp
Ngoài những xử lý dưới đây thì không được phép viết trong Controller
* Nhận Request
* Bind request vs Form
* Chuyển giao xử lý cho Repository
* Chuyển giao xử lý cho Service
* Chuyển đổi dữ liệu
* Render View


### Controller
* Trong trường hợp tạo mới và edit mà lấy các Entity khác nhau thì chia làm 2 method khác nhau trong Controlelr

### Repository
* Trong trường hợp muốn chạy câu query phức tạp mà sử dụng QueryBuilder
* Không dùng `findByXxx()`
* Không tạo `findByXxx()`

### FormType
* Mở rông thì dùng FormEvent / FormExtension
* Chuyển đổi thì dùng DataTransformer

### Service
* Thực thi business logic.

## Unit test
* Cần viết test cho Web、Service、Repository、FormType
* WebTest thì kế thừa từ `AbstractWebTestCase.php`
