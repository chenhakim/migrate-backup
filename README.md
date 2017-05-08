# chenhakim
yii2 migration 备份数据库数据与恢复


### 原作者有话说：

yii2使用migration备份和还原数据库
===========================
yii2使用migration备份和还原数据库，最初只想做一个在命令行中备份的功能，后来将类重组了，增加了其他扩展使用方法。


安装 Installation
------------

安装此扩展的首选方式是通过 [composer](http://getcomposer.org/download/).

运行

```
composer require --prefer-dist chenhakim/chenhakim "*"
```

或者添加

```
"chenhakim/chenhakim": "*"
```

到 `composer.json` 文件的对应地方.


命令行中备份数据表：
-----

在```console\config\main.php```中添加  :

```php
'controllerMap' => [
    'migrates' => [
        'class' => 'chenhakim\migration\ConsoleController',
    ],
],
```

在命令行中使用方式：
```
php ./yii migrate/backup all #备份全部表
php ./yii migrate/backup table1,table2,table3... #备份多张表
php ./yii migrate/backup table1 #备份一张表

php ./yii migrate/up #恢复全部表
```

在后台中备份数据表：
-----
在后台的控制器中，例如```PublicController```中加入下面的代码：
```php
public function actions()
{
    return [
        'backup' => [
            'class' => 'chenhakim\migration\WebAction',
            'returnFormat' => 'json',
            'migrationPath' => '@console/migrations'
        ]
    ];
}
```
在后台中发送一个ajax请求到```/admin/public/backup?tables=yii2_ad,yii2_admin```即可。

其他使用方法：
-----

对于想做更多扩展的朋友，可以直接继承```chenhakim\migration\components\MigrateCreate```

或者使用一下代码：
```php
$migrate = Yii::createObject([
        'class' => 'chenhakim\migration\components\MigrateCreate',
        'migrationPath' => $this->migrationPath
]);
$migrate->create($table);
```


-----
备注：改代码是在e282486518的基础上修改更新的：

新增修改代码，备份为`batchInsert`