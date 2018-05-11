---
layout: default
title: プラグインのテスト
---

---

# {{ page.title }}

## Create a Test 

- Even when make a Plugin, if we inherit test class of EC-CUBE , we can create unit test easily.

- In this chapter, I use simple plugin as an example to explain how to use continuous integration when "Creating a test and push it on  repository"

### Goal of Test

- Check that doest it operate in version of main EC-CUBE.

- When improve plugin used continous, we check dose it run in other version in the pass of EC-Cube.

### Unit test order

1. Create code of test in local.

1. Carry out Unit test in local and check that is there any problems

1. Push to personal GitHub repository

1. Use "Travis" to test in order to check does it meet with EC-CUBE 3 target environment or not.

### Prerequisite

1. Plugin has been created first then check action of install・uninstall and basic function.

1. This time, only create plugins as examples below.

1. Please clone the following and refer.
  - <a href="https://github.com/geany-y/ExamleTest" target="_blank">ExampleTestプラグイン</a>

1. Test target this time is service class [ExampleService.php] of above repository.

### File function of Test target

1. The method [getPluginInstallDateFormatJa] in [ExamleService.php] is the target method.

1. In case passing plugin code as an argument, get the date this plug-in was installed from ** dtb_plugin **.

1. If do not find out plugin had suitable code, return false.

### Create test file

1. Please create folder as below.

  - /app/Plugin/[folder name of Plugin you made]/Tests/Service
  - Because way to create folder changes by the environment, I will omit it.
  - Inside the folder is similar with root directory structure of Plugin, if there is Service, please create in Service folder
  - Example this time is created directly in the folder below

  ---

  ![テストフォルダの作成](images/img-plugin-test-create-folder.png)

  ---

2. Create file 

  - Please create base on the following order.
  - In this example, because we create only the service test, please copy / rename the following files.
    - [EC-CUBEインストールディレクトリ]/tests/Eccube/Tests/Service/ShoppingServiceTest.php
  - After copying, delete all but except methods of initialization・ending process such as setUp and tearDown.

3. Copy to **/app/Plugin/[folder name of Plugin you made]/Tests/Service** of file had been created below.

  - Add corrections and methods as below.

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/plugin_test/ExampleServiceTest.php"></script>

<!--
```
<?php

/*
 * This file is part of the ExampleTest
 *
 * Copyright (C) 2016 LockOn
 *
 * For the full copyright and license information, please view the LICENSE
 * file that was distributed with this source code.
 */

namespace Plugin\ExampleTestPlugin\Tests\Service; ★テストファイルの名前空間を記述

use Eccube\Tests\EccubeTestCase;
use Plugin\ExampleTestPlugin\ServiceProvider\ExampleTestServiceProvider; ★テスト対象ファイルの名前空間定義

class ExampleServiceTest extends EccubeTestCase ★クラス名称を修正
{
    public function setUp() ★テスト開始時に行う処理があれば記述
    {
        parent::setUp();
    }

    /**
     * プラグインのインストール時間取得メソッド失敗パターンのテスト
     * ・インストールされていないコードをサービスに渡す
     * ・戻り値としてfalseが返却される
     */
    public function testGetPluginInstallDateFormatJaFromErrorCode() ★まず正常系エラーのメソッドを追記します
    {
        $errorCode = 'Test'; ★インストールされていないプラグインコードを設定
        $this->actual = $this->app['eccube.plugin.service.example']->getPluginInstallDateFormatJa($errorCode); ★取得値はactualに格納

        $this->assertFalse($this->actual); ★falseが返却される事を定義
    }

    /**
     * プラグインのインストール時間取得メソッド成功パターンのテスト
     * ・インストールされているコードをサービスに渡す
     * ・事前にメソッドと同じ条件でデーターベースからインストール日付を取得しておく
     * ・戻り値としてインストール日付が返却される
     */
    public function testGetPluginInstallDateFormatJaFromSuccessCode() ★次は正常系の正常値テストのメソッドを追記します
    {
        $successCode = 'ExampleTest'; ★今回インストールしたプラグインのコードを記述します

        $qb = $this->app['orm.em']->createQueryBuilder(); ★テスト対象のサービスで取得する値を手動で取得します。
        $qb->select('p.create_date')
            ->from('\Eccube\Entity\Plugin', 'p')
            ->where('p.code = :Code')
            ->setParameter('Code', $successCode);

        try {
            $date = $qb->getQuery()->getSingleResult();
            $this->expected = $date['create_date']->format('Y年m月d日 H時i分s秒'); ★比較値をexpectedに格納します
        } catch (\NoResultException $e) {
            throw new \NoResultException();
        }

        $this->actual = $this->app['eccube.plugin.service.example']->getPluginInstallDateFormatJa($successCode); ★取得値をactualに格納します

        $this->assertEquals($this->actual, $this->expected); ★actualとexpectedが同一である事を定義
    }
}
```
-->

- I will explain the aboved content.

    1. After copying, modify the namespace of the edit file. 
    - Please specify the storage folder for created file this time.

    1. Specify the namespace of ExampleService for this test.

    1. Change the class name to match the test target file this time.

    1. Add method.
    - Firstly, add error test of normal system. 
    - Next, add test of normal system.

#### Note

  - In this chapter, we do not explain how to write test code at all.
  - Please refer the followings to write.
  - <a href="http://qiita.com/chihiro-adachi/items/f2fd1cbe10dccacb3631" target="_blank">EC-CUBE 3のメモ - ユニットテスト -</a>

### Do test at local

- With works until now, we can create test.
- Check once at local then check are there any problems as function in a single environment.

  1.Run the console 

  - Please run the console that suit to your own environment.
  - ※If it is Windows environment, the execution path of PHP have been specified in environment path.

  2. Please move to **EC-CUBE 3のインストールディレクトリ** as below

---

![EC-CUBE 3インストールディレクトリ](images/img-plugin-test-open-console.png)

---

  3. Run the command below.

```
vendor/bin/phpunit ./app/Plugin/[自身で作成したプラグインのフォルダ名]
```

- If content is right, it will be displayed as below.


---

![ユニットテスト結果](images/img-plugin-test-unit-result.png)

---

### Test in multiple environment using continuous integration

- If there is no problem in the previous section, we will push to Github then test in multiple environments with "Travis-CI" which provides continuous integration.

#### Create file for setting Travis-CI

1.Create **.travis.yml** in the root directory of the plugin.

2.In this example, the following folder is applicable.

  - [EC-CUBE 3インストールディレクトリ]/app/Plugin/ExampleTest

3.Create a file in the folder and write the followings.

  - Only parts that need to be changed are marked with ★ and explained.

  - .travis.yml

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/plugin_test/Travis.yml"></script>

<!--
```

language: php

sudo: false

cache:
  directories:
    - $HOME/.composer/cache
    - /home/travis/.composer/cache

php: ★テスト対象のPHPバージョンを指定します
  - 5.3
  - 5.4
  - 5.5
  - 5.6
#  - 7.0

env:
  # plugin code
  global:
    PLUGIN_CODE=ExampleTest ★作成したプラグインのコードを指定します
  matrix:
#    # ec-cube master ★EC-CUBE 3のバージョンを指定します
#    - ECCUBE_VERSION=master DB=mysql USER=root DBNAME=myapp_test DBPASS=' ' DBUSER=root
#    - ECCUBE_VERSION=master DB=pgsql USER=postgres DBNAME=myapp_test DBPASS=password DBUSER=postgres
    - ECCUBE_VERSION=3.0.9 DB=mysql USER=root DBNAME=myapp_test DBPASS=' ' DBUSER=root
    - ECCUBE_VERSION=3.0.9 DB=pgsql USER=postgres DBNAME=myapp_test DBPASS=password DBUSER=postgres
    - ECCUBE_VERSION=3.0.10 DB=mysql USER=root DBNAME=myapp_test DBPASS=' ' DBUSER=root
    - ECCUBE_VERSION=3.0.10 DB=pgsql USER=postgres DBNAME=myapp_test DBPASS=password DBUSER=postgres

matrix:
  fast_finish: true

before_script: ★Travisを初期化しています
  # archive plugin
  - tar cvzf ${HOME}/${PLUGIN_CODE}.tar.gz ./*
  # clone ec-cube
  - git clone https://github.com/EC-CUBE/ec-cube.git
  - cd ec-cube
  # checkout version
  - sh -c "if [ ! '${ECCUBE_VERSION}' = 'master' ]; then  git checkout -b ${ECCUBE_VERSION} refs/tags/${ECCUBE_VERSION}; fi"
  # update composer
  - composer selfupdate
  - composer install --dev --no-interaction -o
  # install ec-cube
  - sh eccube_install.sh ${DB} none
  # install plugin
  - php app/console plugin:develop install --path=${HOME}/${PLUGIN_CODE}.tar.gz
  # enable plugin
  - php app/console plugin:develop enable --code=${PLUGIN_CODE}

script: ★プラグインをインストールしています
  # exec phpunit on ec-cube
  - phpunit app/Plugin/${PLUGIN_CODE}/Tests

after_script: ★プラグインの、インストール・アンインストール、有効化・無効化のテストを行なっています
  # disable plugin
  - php app/console plugin:develop disable --code=${PLUGIN_CODE}
  # uninstall plugin
  - php app/console plugin:develop uninstall --code=${PLUGIN_CODE}
  # re install plugin
  - php app/console plugin:develop install --code=${PLUGIN_CODE}
  # re enable plugin
  - php app/console plugin:develop enable --code=${PLUGIN_CODE}
```
-->

- I will explain the content of setting items above.

1. [php：]
  - Setting versions of php.
  - As the number of combinations increases, it is better to narrow down the version such as 5.3 and 5.6 only.

1. [env：> global：]
  - Specify code of the created plugin on the right side of PLUGIN_CODE.

1. [env：> matrix：]
  - It is specification of the test matrix.
  - Setting versions of EC-CUBE here.

1. [befoe_script：]
  - Preparation before doing Unit test for plugin
    - The processing flow is as below.
      - Packaging plugins (Archive with tar)
      - Clone the ec-cube main
      - checkout for version of ec-cube main body specified by env
      - Install ec-cube main unit
      - Install plugin

1. [script：]
  - Run php unit test.
  - Do unit test that included in the installed plugin.

- We write about setting file of Travis for reference, please refer.

- <a href="https://github.com/EC-CUBE/coupon-plugin/blob/master/.travis.yml" target="_blank">.travis.yml(参考)</a>

#### Linking between Travis-CI and GitHub

- After logging in to GitHub, access the following and turn ON linking.

- `https://travis-ci.org/profile/[user]` 

- From the displayed repository list, slide the suitable repository button. If it become ON grren, linking is completed.


#### Push to GitHub

- After finish, push to your own repository then Travis-CI will automatically run and test. 

- Can confirm test result on the page which GitHub and Travis-CI are set up.
