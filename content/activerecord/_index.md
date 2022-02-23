+++
title = "Playing with ActiveRecord"
description = ""
weight = 16
+++

We want to improve our code by introducing active record concept. For this we will need a library available at https://github.com/bephp/activerecord. We want to replace the SQL queries with a use of the library.

# Hints

To use it you need to execute the following steps:

1. Import it using composer ```composer require bephp/activerecord```
1. Load it into the project using `require __DIR__ . '/vendor/autoload.php';`
1. Define a model
```php
class Friend extends ActiveRecord{
	public $table = 'friend';
    public $primaryKey = 'id';
}
```
1. Connect to the database 
```php
ActiveRecord::setDb(new PDO('sqlite:friends.db'));
```
1. Afterwards use documentation from the [github](https://github.com/bephp/activerecord) to learn how to list all friends as well as add, update and delete them.
1. You should be able to obtain the following working functionality:



