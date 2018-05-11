---
layout: default
title: Let’s create Entity file for Doctrine
---

---

# {{ page.title }}

## Menu of this chapter

- In this chapter, conduct the following part

    1. Explain Entity

    1. Explain about Entity and Entity Manager

    1. Explain about the creating method of Entity file

    1. Explain about  thing that can create Entity file automatically from command line

## Entity

- In this chapter, I created file **Eccube.Entity.[エンティティ名].dcm.yml** in order to mapping database and Entity

- In this chapter, I will create Entity file

### Summary of Entity

- **エンティティファイル** is the simple file that just create column of database by **private** scope as member variable of class, and create **「Setter・Getter」** for that
- If there is Association (Relation Object), that setting will also described.

- As explaining in previous chapter, **エンティティマネージャー** will **エンティティを管理**(manage Entity), store value that occurred in Program on Entity.

- •	If developer **簡単な条件の情報取得**(get info of simple condition), can **マジックメソッドで情報取得**(get info by Magic Method), if developer save info, **対象エンティティに値をセット**(set value in the target Entity) and **保存のメソッドを呼び出すだけ**(just call the saved method)

- **エンティティ**(Entity) is *persist** in Program, first put **エンティティマネージャーの管理下**(under management of Entity)

- When **エンティティをインスタンス化** (instantiate Entity) newly in Program, conduct **persist**, put 
 **エンティティマネージャーの管理下** (under management of Entity)

#### Create file

- Create in the following folder

    - /src/Eccube/Entity

    1. Copy file **AuthorityRole.php** of Folder, rename.


    1. File name will set that **Crud.php**
        - **Crud.php** (contents is AuthorityRole.php)

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_8/crud_entity_before.php"></script>

<!--
```
<?php

namespace Eccube\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * AuthorityRole
 */
class AuthorityRole extends \Eccube\Entity\AbstractEntity
{
    /**
     * @var integer
     */
    private $id;

    /**
     * @var string
     */
    private $deny_url;

    /**
     * @var \DateTime
     */
    private $create_date;

    /**
     * @var \DateTime
     */
    private $update_date;

    /**
     * @var \Eccube\Entity\Master\Authority
     */
    private $Authority;

    /**
     * @var \Eccube\Entity\Member
     */
    private $Creator;


    /**
     * Get id
     *
     * @return integer
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * Set deny_url
     *
     * @param string $denyUrl
     * @return AuthorityRole
     */
    public function setDenyUrl($denyUrl)
    {
        $this->deny_url = $denyUrl;

        return $this;
    }

    /**
     * Get deny_url
     *
     * @return string
     */
    public function getDenyUrl()
    {
        return $this->deny_url;
    }

    /**
     * Set create_date
     *
     * @param \DateTime $createDate
     * @return AuthorityRole
     */
    public function setCreateDate($createDate)
    {
        $this->create_date = $createDate;

        return $this;
    }

    /**
     * Get create_date
     *
     * @return \DateTime
     */
    public function getCreateDate()
    {
        return $this->create_date;
    }

    /**
     * Set update_date
     *
     * @param \DateTime $updateDate
     * @return AuthorityRole
     */
    public function setUpdateDate($updateDate)
    {
        $this->update_date = $updateDate;

        return $this;
    }

    /**
     * Get update_date
     *
     * @return \DateTime
     */
    public function getUpdateDate()
    {
        return $this->update_date;
    }

    /**
     * Set Authority
     *
     * @param \Eccube\Entity\Master\Authority $authority
     * @return AuthorityRole
     */
    public function setAuthority(\Eccube\Entity\Master\Authority $authority = null)
    {
        $this->Authority = $authority;

        return $this;
    }

    /**
     * Get Authority
     *
     * @return \Eccube\Entity\Master\Authority
     */
    public function getAuthority()
    {
        return $this->Authority;
    }

    /**
     * Set Creator
     *
     * @param \Eccube\Entity\Member $creator
     * @return AuthorityRole
     */
    public function setCreator(\Eccube\Entity\Member $creator)
    {
        $this->Creator = $creator;

        return $this;
    }

    /**
     * Get Creator
     *
     * @return \Eccube\Entity\Member
     */
    public function getCreator()
    {
        return $this->Creator;
    }
}

```
-->

#### Modify file

- There is no place that can re-use in case of Entity file 
- Description under contents relating to [create_date/update_date] will be nearly re-written.
- Below is the re-writing contents.

<script src="http://gist-it.appspot.com/https://github.com/EC-CUBE/ec-cube.github.io/blob/master/Source/tutorial_8/crud_entity_after.php"></script>

<!--
```
<?php

namespace Eccube\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * Class Crud
 * @package Eccube\Entity
 */
class Crud extends \Eccube\Entity\AbstractEntity
{
    /**
     * @var integer
     */
    private $id;

    /**
     * @var string
     */
    private $reason;

    /**
     * @var string
     */
    private $name;

    /**
     * @var string
     */
    private $title;

    /**
     * @var string
     */
    private $notes;

    /**
     * @var \DateTime
     */
    private $create_date;

    /**
     * @var \DateTime
     */
    private $update_date;

    /**
     * Get reason
     *
     * @return string
     */
    public function getReason()
    {
        return $this->reason;
    }

    /**
     * Set reason
     *
     * @param $reason
     */
    public function setReason($reason)
    {
        $this->reason = $reason;
    }

    /**
     * Get name
     *
     * @return string
     */
    public function getName()
    {
        return $this->name;
    }

    /**
     * Set name
     *
     * @param $name
     */
    public function setName($name)
    {
        $this->name = $name;
    }

    /**
     * Get title
     *
     * @return string
     */
    public function getTitle()
    {
        return $this->title;
    }

    /**
     * Set title
     *
     * @param $title
     */
    public function setTitle($title)
    {
        $this->title = $title;
    }

    /**
     * Get notes
     *
     * @return string
     */
    public function getNotes()
    {
        return $this->notes;
    }

    /**
     * Set notes
     *
     * @param $notes
     */
    public function setNotes($notes)
    {
        $this->notes = $notes;
    }

    /**
     * Set create_date
     *
     * @param \DateTime $createDate
     */
    public function setCreateDate($createDate)
    {
        $this->create_date = $createDate;

        return $this;
    }

    /**
     * Get create_date
     *
     * @return \DateTime
     */
    public function getCreateDate()
    {
        return $this->create_date;
    }

    /**
     * Set update_date
     *
     * @param \DateTime $updateDate
     */
    public function setUpdateDate($updateDate)
    {
        $this->update_date = $updateDate;

        return $this;
    }

    /**
     * Get update_date
     *
     * @return \DateTime
     */
    public function getUpdateDate()
    {
        return $this->update_date;
    }
}
```
-->


### Create Entity file by command line

- If **エンティティ** is **Eccube.Entity.[エンティティ名].dcm.yml**, can generate automatically from command line

- In this chapter, I will also add method that use command line to create **CrudEntity**.

- If create manual, human error may occur, so I recommend that **自動作成**(create automatically) by **コンソール** as possible.

- After **EC-CUBE 3のインストールディレクトリに移動**(moved to Install Directory of EC-CUBE 3) by command line, than **コマンドを実行**(run command) as below.

※Specify path of Entity in the first row of **Eccube.Entity.[エンティティ名].dcm.yml**. In this time, if **[エンティティ名]**](Entity name) that specified by YAML file name) and **エンティティパスのエンティティ名**(Entity name of Entity path) are different, , it will be **エラー**(error), so please **同一名称を使用**(use same name) for **ファイル名、パスのエンティティ名**.

※**PHPの実行パス**(implementation path of PHP) will set that **環境変数に設定済み** (set in environment environment already)

```

vendor/bin/doctrine orm:generate:entities --extend="Eccube\\Entity\\AbstractEntity" src --filter="Crud"

```

- •	This is reference, if Table creation has **Eccube.Entity.[エンティティ名].dcm.yml**, you can also run from command line similarly.

- About detail, please refer the following part

<a href="http://sssslide.com/speakerdeck.com/amidaike/ec-cube3kodorideingu-number-3" target="_blank">EC-CUBE 3コードリーディング #3</a>

## Study in this chapter

1. Explain summary of internal structure of Entity file
1. Explain relation of Entity file and Entity Manager
1. Explain the creating method that used command line of Entity file
1. Explain the creating method of Table that used command line
