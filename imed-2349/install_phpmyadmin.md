## Install phpMyAdmin

The tool phpMyAdmin allows for easier MySQL/MariaDB database management.  As such, we'll install phpMyAdmin on our web server.

- To begin, we are going to update the package lists by running the command `sudo apt update`

- Next, we'll download and install phpMyAdmin by running the command `sudo apt install phpmyadmin`.  When it asks if you want to continue, type `Y` and hit Enter.

- When you see the screen below, hit the Spacebar to put an asterisk by apache2 and then press the Tab key to highlight the `<OK>` button and hit Enter.

![Configuration screen for choosing the web server for phpMyAdmin](http://inspiringweb.org/moodle/imed2349/phpmyadmin_configuration_web_server.png)

- After it installs multiple packages, you'll see another configuration screen like the one below stating that phpMyAdmin requires a database installed and configured before it can be used.  We'll configure the database using dbconfig-common, so make sure sure that `<Yes>` is selected and hit Enter.

![Configuration screen for choosing to configure a database for phpMyAdmin using dbconfig-common](http://inspiringweb.org/moodle/imed2349/phpmyadmin_configuration_database.png)

- Next, it will display another configuration screen.  This password is only used internally by phpMyAdmin to communicate with MySQL.  Since it isn't a password we will be using routinely, go ahead and enter a random password (make sure you make a note of it because you'll have to re-enter it here in a moment) and hit Enter.

![Configuration screen for entering a password for phpMyAdmin to communicate with MySQL](http://inspiringweb.org/moodle/imed2349/phpmyadmin_configuration_database_password.png)

- Then, you'll see a screen like the one below.  It will then ask you to confirm the password, so re-enter the password you entered on the previous screen and hit Enter.

![Configuration screen for confirming the password for phpMyAdmin to communicate with MySQL](http://inspiringweb.org/moodle/imed2349/phpmyadmin_configuration_password_confirmation.png)

- The installation will finish up.

# Create MySQL superuser

Because we disabled remote login when we installed MySQL, we are going to create a superuser just for phpMyAdmin.

- To create a superuser for just phpMyAdmin, log into MySQL as `root` using the command `sudo mysql -u root -p`

- It will then ask for the password.  If you used the password in the MySQL installation guide, then the password is `!WDDrules19`. After you type it in, remember you won't see any characters on screen, hit Enter.

- Next we are going to add a new MySQL user called `pmauser` (which stands for PHP My Admin User).  In a production environment, you would want your `root` MySQL user and your `pmauser` MySQL user to have different passwords, but for our purposes, we are going to use the same password for both. To create the user, run the query `CREATE USER 'pmauser'@'%' IDENTIFIED BY '!WDDrules19';`.  You should get the result:  
  `Query OK, 0 rows affected (0.00 sec)`

- Then we'll grant superuser privileges to our new user `pmauser`.  To do this, run the command `GRANT ALL PRIVILEGES ON *.* TO 'pmauser'@'%' WITH GRANT OPTION;`.  You should see the result of this query as:  
  `Query OK, 0 rows affected (0.00 sec)`

- Now exit MySQL.

## Test phpMyAdmin

Now, we should be able to access and log into the phpMyAdmin web interface.

- In your browser, enter your server's IP address followed by `/phpmyadmin`. e.g. `http://192.168.1.18/phpmyadmin`

- You should see the login form shown below.

![Login screen for phpMyAdmin](http://inspiringweb.org/moodle/imed2349/phpmyadmin_login_screen.png)

- Enter the the MySQL user `pmauser` in the username field and then `!WDDrules19` in the password field then click __Go__.  Once you are logged in, you should see a screen similar to the one below.

![Web Interface for phpMyAdmin](http://inspiringweb.org/moodle/imed2349/phpmyadmin_web_interface.png)
