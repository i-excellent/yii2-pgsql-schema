yii2-pgsql
==============

Improved PostgreSQL schemas for Yii2.

Supports follow types for ActiveRecord models:

[![Latest Stable Version](https://poser.pugx.org/Tigrov/yii2-pgsql/v/stable)](https://packagist.org/packages/Tigrov/yii2-pgsql)
[![Build Status](https://travis-ci.org/Tigrov/yii2-pgsql.svg?branch=master)](https://travis-ci.org/Tigrov/yii2-pgsql)

Limitation
------------

When you use this extension you can't specify the PDO type by using an array: `[value, type]`,

e.g. `['name' => 'John', 'profile' => [$profile, \PDO::PARAM_LOB]]`.

See the issue [#7481](https://github.com/yiisoft/yii2/issues/7481)

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist i-excellent/yii2-pgsql-schema
```

or add

```
"i-excellent/yii2-pgsql-schema": "~1.0"
```

to the require section of your `composer.json` file.

 
Configuration
-------------
Once the extension is installed, add following code to your application configuration:

```php
return [
    //...
    'components' => [
        'db' => [
            'class' => 'yii\db\Connection',
            'dsn' => 'pgsql:host=localhost;dbname=<database>',
            'username' => 'postgres',
            'password' => '<password>',
            'schemaMap' => [
                'pgsql'=> 'i-excellent\pgsql-schema\Schema',
            ],
        ],
    ],
];
```



Configure Model's rules
```php
/**
 * @property string[] $attribute1 array of string
 * @property array $attribute2 associative array or just array
 * @property integer|string|\DateTime $attribute3 for more information about the type see \Yii::$app->formatter->asDatetime()
 */
class Model extends ActiveRecord
{
    //...
    public function rules()
    {
        return [
            [['attribute1'], 'each', 'rule' => ['string']],
            [['attribute2'], 'safe'],
        ];
    }
}
```
	
Usage
-----

You can then save array, json and timestamp types in database as follows:

```php
/**
 * @var ActiveRecord $model
 */
$model->attribute1 = ['some', 'values', 'of', 'array'];
$model->attribute2 = ['some' => 'values', 'of' => 'array'];
$model->save();
```

and then use them in your code
```php
/**
 * @var ActiveRecord $model
 */
$model = Model::findOne($pk);
$model->attribute1; // is array
$model->attribute2; // is associative array (decoded json)

```

[Composite types](docs/composite.md)

License
-------

[MIT](LICENSE)
