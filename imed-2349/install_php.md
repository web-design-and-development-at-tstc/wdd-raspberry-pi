## Install Apache Utilities

- First, we are going to install some useful Apache utilities.  Run the command, `sudo apt intstall apache2-utils`

## Add PHP 7.3 PPA

- We need to add a PPA repository to allow us to install the latest version of PHP.  To do this, run the command `sudo add-apt-repository ppa:ondrej/php`

- Next, we are going to add a PPA repository for apache 2.  Run the command `sudo add-apt-repository ppa:ondrej/apache2`

## Install PHP

- Now that we have the PPA repositories set up, we can install PHP 7.3 onto our web server.  To do this, run the command `sudo apt-get install php7.3`

- Next, run the command `sudo apt install php7.3-cli php7.3-fpm php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-gd  php7.3-mbstring php7.3-curl php7.3-xml php7.3-bcmath php7.3-json`

- You can confirm the installed version of PHP extension using the command `apt policy php7.3-cli`

## Verify PHP is Working

- In the Terminal window, run the command `sudo nano /var/www/html/info.php`

- In the Nano editor, write the HTML code to create a basic HTML document and in the body of the web page include the PHP code, `<?php phpinfo(); ?>`

- Once you are finished writing the code, press CTRL + X, then hit `Y`, and then enter.

- Open up your browser and go to your IP_ADDRESS/info.php (make sure you replace the IP_ADDRESS with the IP address of your Raspberry Pi).  You should see the PHP Info page showing the version of PHP you have installed and a bunch of configuration information.
