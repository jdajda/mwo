+++
title = "Connecting to DB"
description = ""
weight = 11
+++

We have the following SQLite database:

![](/db1.png)

We want to display names of the friends in a list. Look at the example:

![](/db2.png)   

```php
class MyDB extends SQLite3 {
    function __construct() {
       $this->open('friends.db');
    }
 }

 $db = new MyDB();
 if(!$db) {
    echo $db->lastErrorMsg();
    exit();
 } 

 echo "<h2>My friends</h2>";
 $sql = "select * from friend";
 $ret = $db->query($sql);
 while($row = $ret->fetchArray(SQLITE3_ASSOC) ) {
     echo $row['name'] . " <br>";
 }

 $db->close();
```

You will need the database file. Download the file [friends.db](/friends.db). To see the database you can use an online tool called https://sqliteonline.com. You need to load the database file there to see such result:

![](/sqliteonline.png) 

# In case of problems

***If you are working on laboratory computers you don't need to configure anything here :-)***

It is possible that you will encounter errors during executing the above code. In this case you need to enable your sqlite extension in php.ini config. 

Follow the steps:

1. Find your ```php.ini``` file. 
   1. On Windows machines you can find it in ```C:\xampp\php\php.ini``` or  ```C:\Windows``` paths
   2. On MacOS or Linux systems find it using the following command ```php -i | grep php.ini```
1. Edit the file. Find line ```;extension=php_sqlite3.dll``` and remove the first character (semicolon)
1. Restart the server. It should work
1. Sometimes when you install your PHP to a custom folder (for example in ```Program Files``` on Windows) PHP cannot find the extension directory. In this case you need to provide the correct extension directory in ```php.ini``` file. To this purpose edit parameter ```extension_dir``` to point to proper folder. For example 
   ```
   extension_dir="C:\xampp\php\ext"
   ```