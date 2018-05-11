---
layout: default
title: Let's create Database
---

---

# Let's create Database

## Menu of this chapter

- In this chapter, conduct the following par

    1. Explain about the creation method of Migration file that describe Table definition

    1. Explain about definition of Table that uses in this Tutorial

    1. Explain about method that use **エンティティマネージャー** to create Table

    1. Explain about the thing that needs to register data in dtb_page_layout to display Header Footer of Template file

    1. Explain about method that register screen info with **dtb_page_layout**

## Table definition of this tutorial

- As described in Menu of this chapter,I will explain about contents of Table definition

<!-- テーブル定義は**マイグレーションファイル**に記述していきます。 -->
- Use Entity Manager in **マイグレーションファイル**(Migration file) to extract Table schema from Entity, create Table

## Preparation of Migration file

1. Based on contents of [マイグレーション作成手順(Migration creating Manual)] of [マイグレーションガイド](Migration Guide) that is described in TOP-PAGE, to create new Migration file. 

    - If conduct generation based on contents,it will be created in the following folder.
    - The store folder
      - /src/Eccube/Resource/doctrine/migration

1. Please confirm whether the following file has finished or not?
    - **Version20160607155514.php**

    - Describe contents in below
    - Method of [up] and [down] has been describing

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_6/migration_before.php"></script>

<!--
```
<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

/**
 * Auto-generated Migration: Please modify to your needs!
 */
class Version20160607155514 extends AbstractMigration
{
    /**
     * @param Schema $schema
     */
    public function up(Schema $schema)
    {
        // this up() migration is auto-generated, please modify it to your needs
        ★ここにテーブル定義を追記

    }

    /**
     * @param Schema $schema
     */
    public function down(Schema $schema)
    {
        // this down() migration is auto-generated, please modify it to your needs
        ★ここのテーブル定義を削除

    }
}
```
-->

## Table definition of Tutorial of this time

- Table name: dtb_crud

| Logical name | Physical name | Field name | Others |
|------|------|------|------|
| Submission ID | id | int | NOT NULL PRIMARY AUTO_INCREMENT |
| Submission Type | reason | smallint | NOT NULL |
| Submission persion handle name | name | varchar(255) | NOT NULL |
| Title of submission | title | varchar(255) | NOT NULL |
| Submission type | notes | text | DEFAULT NULL |
| Submission registration time | created_date | datetime | NOT NULL |
| Submission editing time | updated_date | datetime | NOT NULL |

- This is table that created part above

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_6/migration_after.php"></script>

<!--
```

<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

use Doctrine\ORM\Tools\SchemaTool; ★テーブルを作成するために利用します
use Eccube\Application; ★エンティティマネージャーの取得のために必要です

/**
 * Auto-generated Migration: Please modify to your needs!
 */
class Version20160607155514 extends AbstractMigration
{

    /**
     * @param Schema $schema
     */
    public function up(Schema $schema)
    {
        if (!$schema->hasTable('dtb_crud')) {
            $entities = array(
                'Eccube\Entity\Crud' ★テーブル作成を行うエンティティを指定します
            );
            $app = Application::getInstance(); ★エンティティマネージャーの取得のためにApplicationを取得します
            $em = $app['orm.em']; ★エンティティマネージャーを取得します
            $classes = array();
            foreach ($entities as $entity) {
                $classes[] = $em->getMetadataFactory()->getMetadataFor($entity); ★エンティティからカラム情報を取得します。
            }
            $tool = new SchemaTool($em); ★テーブル生成のためにスキーマツールをインスタンス化します
            $tool->createSchema($classes); ★テーブルを生成します
        }
    }

    /**
     * @param Schema $schema
     */
    public function down(Schema $schema)
    {
        if (!$schema->hasTable('dtb_crud')) {
            $schema->dropTable('dtb_crud');
        }
    }
```
-->

- I will explain about above

    1. **$schema** variable exists in parameter of method, but it is Object that conducts Table operation.

    1. This contents includes many contents that explain in next chapter, so I just explain this time.
        - Detail will check below of next chapter
    1. iii.	Next, get Instance of 「Application」
        - As explained before, call method of **「$app」** and build Application, this time is also similary.
    1. Get Entity Manager from Instance of Application.
    1. Use method **hasTable** to check **今回テーブルの有無**(there is/no table this time)
    1. Store path of Entity of Table creating target in array. 
        - Because there is one record that corresponding with the created table this time, so it is not esstential to store Entity name in Array. This explanation contents is general, so I am storing in Array.
    1. Specify path of the corresponding Entity this time into **getMetadataFactory** of Entity Manager and **getMetadataFor** of that methods, in order to get column info from Entity.
    1. Transfer Entity Manager to **SchemaTool** by parameter, get Instance.
    1. Transfer the gotten column info to method **createSchema** of  **SchemaTool**, generate table
    1. Next, explain about method [down], method [down] has been deleting table by method **dropTable**

## Registration of screen info to dtb_page_layout

- If screen info **登録されていないと表示されません**(has not been registered) in **dtb_page_layout**, **ヘッダー・フッター**(Header Footer) of Template of EC-CUBE 3 will not be displayed.

- Describe into that saves in **dtb_page_layout** in below

- Table name:  dtb_page_layout

| physical name | registration info | registration value |
|------|------|------|
| device_type_id | Type of display Device | Get key 10 (PC) of mtb_device_type and store |
| page_name | Title of screen | Tutorial/CRUD |
| url | Routing name of screen | tutorial_crud |
| file_name | path and name from Rout of corresponding Twig | Tutorial/crud_top |
| edit_flg | Able to edit from Management screen or not | 2 |

- Describe the record addition of above into below.

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_6/migration_add_dtb_layout.php"></script>

<!--
```

<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

use Doctrine\ORM\Tools\SchemaTool; ★テーブルを作成するために利用します
use Eccube\Application; ★エンティティマネージャーの取得のために必要です

/**
 * Auto-generated Migration: Please modify to your needs!
 */
class Version20160607155514 extends AbstractMigration
{

    /**
     * @param Schema $schema
     */
    public function up(Schema $schema)
    {
        $app = Application::getInstance();
        $em = $app['orm.em'];
        if (!$schema->hasTable('dtb_crud')) {
            $entities = array(
                'Eccube\Entity\Crud'
            );
            $classes = array();
            foreach ($entities as $entity) {
                $classes[] = $em->getMetadataFactory()->getMetadataFor($entity); ★エンティティからカラム情報を取得します。
            }
            $tool = new SchemaTool($em);
            $tool->createSchema($classes);
        }

        $qb = $em->createQueryBuilder(); ★クエリビルダーを取得

        $qb->select('pl') ★該当情報が登録済みかどうかを確認するためのSQLを構築
            ->from('\Eccube\Entity\PageLayout', 'pl')
            ->where('pl.url = :Url')
            ->setParameter('Url', 'tutorial_crud');

        $res = $Point = $qb->getQuery()->getResult(); ★SQL結果を取得

        if(count($res) < 1){ ★結果がなければ、以下情報を書き込み
            $PageLayout = new PageLayout(); ★登録するためのエンティティをインスタンス化
            $DeviceType = $em->getRepository('\Eccube\Entity\Master\DeviceType')->find(10); ★格納するデバイスタイプをDBから取得
            $PageLayout->setDeviceType($DeviceType); ★以下登録エンティティに必要情報を格納
            $PageLayout->setName('チュートリアル/CRUD');
            $PageLayout->setUrl('tutorial_crud');
            $PageLayout->setFileName('Tutorial/crud_top');
            $PageLayout->setEditFlg(2);

            $em->persist($PageLayout); ★エンティティマネージャーの管理化に登録エンティティ追加
            $em->flush($PageLayout); ★登録エンティティを対象に保存
        }
    }

    /**
     * @param Schema $schema
     */
    public function down(Schema $schema)
    {
        if (!$schema->hasTable('dtb_crud')) {
            $schema->dropTable('dtb_crud');
        }

        $app = \Eccube\Application::getInstance(); ★EC-CUBEのアプリケーションクラスを取得
        $em = $app['orm.em']; ★エンティティマネージャーを取得
        $qb = $em->createQueryBuilder(); ★クエリビルダーを取得

        $qb->select('pl') ★該当画面情報が保存されているかを確認するためのSQLを生成
            ->from('\Eccube\Entity\PageLayout', 'pl')
            ->where('pl.url = :Url')
            ->setParameter('Url', 'tutorial_crud');

        $res = $Point = $qb->getQuery()->getResult(); ★情報取得

        if(count($res) > 0){ ★該当情報が保存されていれば、削除処理
            $qb->delete() ★該当画面情報を削除するための、SQLを生成
                ->from('\Eccube\Entity\PageLayout', 'pl')
                ->where('pl.url = :Url')
                ->setParamater('Url', 'tutorial_crud');
            $res = $Point = $qb->getQuery()->execute(); ★削除処理実行
        }
    }
}
```
-->

- When finished above, refer chapter of [マイグレーション受け入れ手順(Migration acceptance manual)] of [マイグレーションガイド(Migration Guide)], and execute that contents.

- If operation succeeded, table has been creating as below.


---

![bbsテーブル](../../../images/img-tutorial6-create-table.png)

---

## Study in this chapter

1. Create empty [マイグレーションファイル(migration file) from Console.
1. Consider Table structure.
1. Explain about method that get column info of Entity from Entity Manager.
1. Explain about method that create table from column info in **SchmeTool** 
1. Conduct database operation by [schemaオブジェクト(schema Object)] of [マイグレーションファイル(migration file)].
1. Use method of 「hasTable」「dropTable」in Object [schema], delete table
1. Explain about in screen that created without registering screen info in dtb_page_layout, Header/ Footer are not displayed.
1. Explain info that register to dtb_page_layout
1. Conduct registration info in dtb_page_layout
