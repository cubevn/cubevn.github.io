---
layout: default
title: Let’s set Database structure for Doctrine
---

---

# {{ page.title }}

## Menu of this chapter

- In this chapter, conduct the following part

    1. Conduct summary of ORM and explanation of Entity Manager. 

    1. Conduct explanation of Entity file and Entity Manager.

    1. Explain relation between Entity Manager and database structure definition file

    1. Explain about creation method of Database structure definition File (Eccube.Entity.[エンティティ名].dcm.yml)


## Database structure definition File

- In this chapter, I created Table of this Tutorial

- In EC-CUBE, use **ObjectRelationalMapping** of **Doctine** for Database operation in order to process Database record transparently

- The general concept is as below

    - o	This is explanation about JPA/Hibernate, I think it is easy to understand about **Doctrineのエンティティマネージャーの概要**(summary of Entity Manager of Doctrine)

        - <a href="http://builder.japan.zdnet.com/sp_oracle/weblogic/35067018/" target="_blank">初めてのJPA--シンプルで使いやすい、Java EEのデータ永続化機能の基本を学ぶ</a>


        - <a href="https://vladmihalcea.com/2014/07/30/a-beginners-guide-to-jpahibernate-entity-state-transitions/" target="_blank">A beginner’s guide to JPA/Hibernate entity state transitions</a>

- In Doctrine, **エンティティマネージャー**(Entity Manager) will **データモデルオブジェクト(エンティティ)を管理**(manage Data Model Object (Entity)) on Programe, while confirm the difference of record, register/ update properly 

- About Entity structure, in order to create file Entity Class actually, you don’t have to express structure in Doctrine, but relation of **エンティティとテーブル**(Entity and Table) will have to create/ express **定義ファイル**(definition file) and **テーブルを対応(Mapping)**(handle (Mapping) Table ) with Entity.

- In this chapter, create file that defines handling (Mapping) Entity with Table

### Create Eccube.Entity.[エンティティ名].dcm.yml

#### Create File

- Create in the following folder.

    - /src/Eccube/Resource/doctrine

    1. Copy file **Eccube.Entity.AuthorityRole.dcm.yml** of Folder, and rename

    2. File name will set that **Eccube.Entity.Crud.dcm.yml**
        - **Eccube.Entity.Crud.dcm.yml**(contents id Eccube.Entity.AuthorityRole.dcm.yml)

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_7/dcm_yml_before.yml"></script>

<!--
```

Eccube\Entity\AuthorityRole:
    type: entity
    table: dtb_authority_role
    repositoryClass: Eccube\Repository\AuthorityRoleRepository
    id:
        id:
            type: integer
            nullable: false
            unsigned: false
            id: true
            column: authority_role_id
            generator:
                strategy: AUTO
    fields:
        deny_url:
            type: text
            nullable: false
        create_date:
            type: datetime
            nullable: false
        update_date:
            type: datetime
            nullable: false
    manyToOne:
        Authority:
            targetEntity: Eccube\Entity\Master\Authority
            joinColumn:
                name: authority_id
                referencedColumnName: id
                nullable: false
        Creator:
            targetEntity: Eccube\Entity\Member
            joinColumn:
                name: creator_id
                referencedColumnName: member_id
                nullable: false
    lifecycleCallbacks: {  }

```
-->

#### Modify file

- Modify above as below

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_7/dcm_yml_after.yml"></script>


<!--
```

Eccube\Entity\Crud: ★エンティティのパスをCrudに変更します( ファイルは後で作成します )
    type: entity
    table: dtb_bbs  ★テーブル名をdtb_crudに修正します
    repositoryClass: Eccube\Repository\CrudRepository ★レポジトリをCrudに修正します( ファイルは後の章で作成します )
    id: ★プライマリーキーの設定を行います
        id:
            type: integer
            nullable: false
            unsigned: false
            id: true
            column: id ★カラム名を修正します
            generator:
                strategy: AUTO ★オートインクリメントを利用するためにAUTOを設定します
    fields: ★カラムの設定を行います
        reason:
            type: smallint
            nullable: false
        name:
            type: string
            lenght: 255
            nullable: false
        title:
            type: string
            lenght: 255
            nullable: false
        notes:
            type: text
            nullable: false
        create_date:
            type: datetime
            nullable: false
        update_date:
            type: datetime
            nullable: false
    lifecycleCallbacks: {  }

```
-->

- Conduct explanation as above

    1. [Eccube\Entity\Crud:]
    - Specify path of Entity file
    - Position of storing file has been decided, so change just file name.

    1. [table:]
    - Specify table name of corresponding table

    1. [repositoryClass:]
     - Specify repository name that linking with this table.
     - It is class for describing Business Logic and conducting database operation
	Create in next chapter

     - Create in next chapter

    1. Conduct setting of Primary key with 「id:」
     - Follow the contents of Table definition that defined in the previous chapter.

    1. Specify physical name of Primary key with option [column:] in session [id:]

    1. Set [AUTO] in「**generator:strategy**」of session 「id:」
    - According to this setting to handle automatically for auto_increment such as MySQL, PostgresSQL
    - Besides, it is possible to set numbering method of ID, but normally set [AUTO] is no problem
    - <a href="http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/basic-mapping.html#identifier-generation-strategies" target="_blank">4.5.1. Identifier Generation Strategies</a>

    1. With [fields:] , conduct setting of normal column
    - Basically, set [typeにはテーブルのフイールドタイプ] (Field type of Table in type) that [nullableは**NOT NULL**であればfalse](if nullable is  **NOT NULL**, it will be false).
    - Besides, there is Option that can set such as [length] of example of this time.
    - <a href="http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/basic-mapping.html#property-mapping" target="_blank">4.3. Property Mapping</a>

    1. This Tutorial is contents using for beginners, so I will not explain about association (setting of relation) 

    1. About detail explanation, please refer the following part. 
    - About Field type of setting file, please refer the following part

    <a href="http://docs.symfony.gr.jp/symfony2/book/doctrine.html#doctrine" target="_blank">http://docs.symfony.gr.jp/symfony2/book/doctrine.html#doctrine</a>


    - About summary of basic setting, please refer below.

    <a href="http://docs.symfony.gr.jp/symfony2/book/doctrine.html#doctrine-the-model" target="_blank">データベースと Doctrine (“The Model”)</a>

#### Remarks

- The following contents is described at the end of definition file

```

lifecycleCallbacks: {  }

```

- Here, specify cut function in process of Doctrine as callback, normal it is not used, so I will omit the explanation.
- Describe conventionally, do not conduct deletion.


## Study in this chapter

1. Explain summary of Doctrine
1. Explain about Doctrine and Entity Manager.
1. Explain about relation of Table, database definition file, Entity, repository.
1. Explain about the creating method of Eccube.Entity.[エンティティ名].dcm.yml
1. Explain about the describing method of Eccube.Entity.[エンティティ名].dcm.yml
