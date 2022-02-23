+++
title = "Adding data to database"
description = ""
weight = 14
+++

Finally, we want to extend our example by allowing user to add new friend. For this purpose we just need to add additional action to the form and handle this new action. Check the example below:

![](/db5.gif)

# Hints
1. Add new button to the form. Use same `name` properties for both buttons: to filtering and adding. Based on its value you will know what action should be executed.
2. Whe you are adding new friends, you must reset form fields to be empty. You can achieve this by changing the value to something like this: `value="<?php if ($action=="Filter friends") echo $name?>"`
3. To add new record to the database use `INSERT` SQL command, like in this example: `$sql = "insert into 'friend' (name, surname, email) values ('$name', '$surname', '$email')";`
