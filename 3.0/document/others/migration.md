---
layout: default
title: マイグレーションガイド
---

---

# {{page.title}}

## Summary

 - Automate reflection to operation environment of DB definition that was changed according to Migration up of EC-CUBE system 3.
 - The change of DB definition means create/ delete Table/ Field/ Index, insert/ update/ delete Record.
 - Migration process will use Doctrime migration Module.
 - By using Doctrime migration Module, compare DB definition that was defined in source code of EC-CUBE with DB definition on DB server, just apply the change of the essential DB definition to DB server.

## Structure of DB Migration process

 - With migration, use Doctrine-migration Module, and use ServiceProvider(**https://github.com/dbtlr/silex-doctrine-migrations**) to call from Silex as a wrapper.
 - Migration process is premise to start from Console at first.
 -	When create Web Installer, you can start from Installer.
 - Migration file will put in src/Eccube/Resource/doctrine/migration

## Manual of creating Migration

 - Modify Table definition file (src/Eccube/Resource/doctrine/) of Doctrine 
  (No need on case there is no change of Table definition)

 - Generate empty Migration file

   $ php app/console migrations:generate

    Generated new migration class to "/var/www/html/ec-cube/src/Eccube/Resource/doctrine/migration/Version20150519204013.php"

 - Describe the change contents in up method definition of Migration file that created above. Process of change can describe by the following way.
   - Operation(such as ($table=$schema->createTable(“foo”)) of Table definition that used SchemaManager
   - Operation (such as ($em->persist($customer)) of record that used EntityManager.
   - Implement SQL that embedded as literal (non-recommended)
 - Similarily, describe the process that needs for roll-back of DB definition, is down method definition.

## Manual of accepting Migration

 - Update source code of EC-CUBE
 - Reflect the created Migration into DB

   $ php app/console migrations:migrate

       Doctrine Database Migrations

       WARNING! You are about to execute a database migration that could result in schema changes and data lost. Are you sure you wish to continue? (y/n)y

   (Nothing is outputted, but it has been reflected. In case DB definition has been tracked already in source code, it will display that Nothing to migrate)

## Confirmation of Migration status 

 - In order to check there is/ no Migration that needs for application and check currently DB definition version, get status of migration

   $ php app/console migrations:status

Version up from system 2 will consider separately...

## DB Migration Guide using from EC-CUBE developer

### Manual of change of Table Definition

#### First of all
About purpose of Migration mechanism, there are 2 kinds of implementation of DDL (change Table definition) and implementation of DML (Initial record definition). Each creating manual will be described below.

#### Manual of creating Migration File using for DDL

1. Generate Migration file

  (php app/console migrations:generate)

2. Describe Manual of change in up,down method of Migration file 

  However, In case contents of change of plan is Implemented already, set in order to ignore (because it will be duplicated with operation of in case run orm:schema-tool:create when clean install)

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/migration/DDLMigration.php"></script>

3. Create/ Edit Table definition file (yml) of doctrine to match with contents of 2

4. Edit entity, repository of doctine to match with contents of 3

5. Run (php app/console migrations:migrate) Migration from command line, check whether become the assumed DB definition or not?

6. Run Clean Install from Web Installer, check whether become DB definition like 5 or not?


#### Manual of creating Migration file using for DML

1. Generate (php app/console migrations:generate) Migration file
  (php app/console migrations:generate)

2. Describe the change manual in up,down method of Migration file
  (In case of DML, no need to check the different implementation contents with DDL)

<script src="https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/migration/DMLMigration.php"></script>

3. Run (php app/console migrations:migrate) Migration from command line, check that the assumed initial data is inserted or not?


## Migration of using Doctrine

### Create Entity file

After update yaml, if run the following part, Entity will be created.  
About the original source will not be delete, just Item that doesn’t exist, will be added.

```
vendor/bin/doctrine orm:generate:entities --extend="Eccube\Entity\AbstractEntity" src
```

### Migration to DB

```
vendor/bin/doctrine orm:schema-tool:update
```

Run above, check whether can migration or not?

```
vendor/bin/doctrine orm:schema-tool:update --dump-sql
```

Run above, check sql

```
vendor/bin/doctrine orm:schema-tool:update --force
```

Run above, Migration to DB
