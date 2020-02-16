## Install Apache Utilities

- First, we are going to install some useful Apache utilities.  Run the command, `sudo apt install apache2-utils software-properties-common`.  These may already be installed and at the latest version.

## Add PHP and Apache2 PPA Repositories

- We need to add a PPA repository to allow us to install the latest version of PHP.  To do this, run the command `sudo add-apt-repository ppa:ondrej/php`.  After it has begun to run, it will ask you to press Enter to continue or Ctrl+C to cancel adding it.  Hit Enter.

- Next, we are going to add a PPA repository for apache 2.  Run the command `sudo add-apt-repository ppa:ondrej/apache2`.  After it has begun to run, it will ask you to press Enter to continue or Ctrl+C to cancel adding it.  Hit Enter.

- After you have added both repositories, run the command `sudo apt-get update`.

## Install PHP

- Now that we have the PPA repositories set up, we can install PHP 7.4 onto our web server.  To do this, run the command `sudo apt install php7.4`.  When it asks if you want to continue, type `Y` and hit Enter.

- When it has finished running, to verify that the installation was successful, run the command `php -v`.

- If PHP was successfully installed, you will see output similar to:

  `PHP 7.4.2 (cli) (built: Feb  5 2020 16:50:55) ( NTS )`  
  `Copyright (c) The PHP Group`  
  `Zend Engine v3.4.0, Copyright (c) Zend Technologies`  
  `    with Zend OPcache v7.4.2, Copyright (c), by Zend Technologies`

- Next, run the command `sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl`. After it has determined what all will be installed, it will ask if you want to continue.  Type `Y` and hit Enter.

## Configure PHP

- To get better performance out of our PHP module, we are going to update several values in the php.ini file.  To open the file, run the command `sudo nano /etc/php/7.4/apache2/php.ini`.

- Press Ctrl+W and type `upload_max_filesize`.  Then press enter.  This will go to the place in the file where this variable is set.  Change it's value to `32m`.

- Press Ctrl+W and type `post_max_size`.  Then press enter.  This will go to the place in the file where this variable is set.  Change it's value to `48M`.

- Press Ctrl+W and type `memory_limit` .  Then press enter.  This will go to the place in the file where this variable is set.  Change it's value to `256M`.

- Press Ctrl+W and type `max_execution_time`.  Then press enter.  This will go to the place in the file where this variable is set.  Change it's value to `600`.

- Press Ctrl+W and type `max_input_vars`.  Then press enter.  This will go to the place in the file where this variable is set.  Remove the semi-colon (`;`) at the start of the line and change it's value to `3000`.

- Press Ctrl+W and type `max_input_time`.  Then press enter.  This will go to the place in the file where this variable is set.  Change it's value to `1000`.

- Once you have set all 6 of these settings, press Ctrl+X, then type `Y`, and then press Enter.

- For these changes to take affect, run the command `sudo systemctl restart apache2.service`.

## Verify PHP is Working

- In the Terminal window, run the command `sudo nano /var/www/html/info.php`

- In the Nano editor, write the HTML code to create a basic HTML document and in the body of the web page include the PHP code, `<?php phpinfo(); ?>`

- Once you are finished writing the code, press CTRL + X, then hit `Y`, and then enter.

- Open up your browser and go to your IP_ADDRESS/info.php (make sure you replace the IP_ADDRESS with the IP address of your Raspberry Pi).  You should see the PHP Info page showing the version of PHP you have installed and a bunch of configuration information.
