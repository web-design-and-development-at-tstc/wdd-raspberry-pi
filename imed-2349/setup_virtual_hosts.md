## Setting Up Virtual Hosts

With Apache, it is possible to host multiple sites/domains on Apache using Virtual Hosts.  Even if you are only going to host a single site/domain, it is still a good idea to set up a directory and Virtual Host. If you ever decide to add another domain later, things will be a lot easier for you.  For this guide, we are not going to use registered domains and we'll set up the hosts file in our OS to resolve the domains we use in this guide.

The first domain we are going to make a virtual host for is `jdoe.dev` (where j is the initial of your first name and doe is your last name; so for example my first domain would be `dtrower.dev`).  The second domain we are going to make a virtual host for is a development domain for your food truck/restaurant.  In this guide, replace `jdoe.dev` with the domain using your name and replace `food_truck.dev` with a development domain for your food truck/restaurant.

## Create Directories and Set Permissions

Inside the `/var/www/` directory we are going to create two new directories for our two domains.  Remember to replace `jdoe.dev` and `food_truck.dev` with your own development domains.

- Run the command `sudo mkdir -p /var/www/jdoe.dev/public_html`

- Then run the command `sudo mkdir -p /var/www/food_truck.dev/public_html`

- Next we'll need to change the permissions of the general web directory `/var/www` and its contents so our websites can be served correctly.  To do this, run the command, `sudo chmod -R 755 /var/www`

## Create Test Web Pages

For each domain we are going to create a simple index.html web page to test that our setup is correct.  Remember to replace `jdoe.dev` and `food_truck.dev` with your own development domains.

- To create the first index.html web page for `jdoe.dev`, run the command `sudo nano /var/www/jdoe.dev/public_html/index.html`

- In the nano editor, Create a simple HTML web page with the your name in the &lt;title&gt; element and inside an &lt;h1&gt; element so we can see something in the tab and the browser window when we load the website.

- To exit the nano editor and save this file, press CTRL+X to exit, type `Y` to save, and Enter to confirm the file name.

- We'll do the same thing for food_truck.dev.  Run the command `sudo nano /var/www/food_truck.dev/public_html/index.html`

- In the nano editor, Create a simple HTML web page with the name of your food truck/restaurant in the &lt;title&gt; element and inside an &lt;h1&gt; element so we can see something in the tab and the browser window when we load the website.

- To exit the nano editor and save this file, press CTRL+X to exit, type `Y` to save, and Enter to confirm the file name.

## Create New Virtual Host Files

The Virtual Host files are used to tell Apache how to respond to various domain requests.   These files are located in `/etc/apache2/sites-available`.

- To begin, we are going to copy the default virtual host file called `000-default.conf` to a new virtual host file for our `jdoe.dev` domain.  Run the command `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/jdoe.dev.conf`

- Next, open the copied file in nano by running the command `sudo nano /etc/apache2/sites-available/jdoe.dev.conf`  We are going to change the lines for ServerAdmin and DocumentRoot and in between add the lines for ServerName and ServerAlias.  Make sure to replace `jdoe.dev` with your own domain.

  `    ServerAdmin webmaster@jdoe.dev`  
  `    ServerName jdoe.dev`  
  `    ServerAlias www.jdoe.dev`  
  `    DocumentRoot /var/www/jdoe.dev/public_html`

- Save and close nano by pressing Ctrl+X, then type `Y` and hit Enter.

- We'll repeat the process for our food truck/restaurant domain.  Run the command `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/food_truck.dev.conf`

- Next, open the copied file in nano by running the command `sudo nano /etc/apache2/sites-available/jdoe.dev.conf`  We are going to change the lines for ServerAdmin and DocumentRoot and in between add the lines for ServerName and ServerAlias.  Make sure to replace `food_truck.dev` with your own domain.

  `    ServerAdmin webmaster@food_truck.dev`  
  `    ServerName food_truck.dev`  
  `    ServerAlias www.food_truck.dev`  
  `    DocumentRoot /var/www/food_truck.dev/public_html`  

  - Save and close nano by pressing Ctrl+X, then type `Y` and hit Enter.

## Enable New Virtual Host files

Now that we have the two virtual host files in place, we need to enable them.  Remember to replace the domains below with your own domains.

- First, run the command `sudo a2ensite jdoe.dev.conf`  You should see the following message:

  `Enabling site jdoe.dev.`  
  `To activate the new configuration, you need to run:`  
  `  systemctl reload apache2`

- Next, run the command `sudo a2ensite food_truck.dev.conf`  You should see the following message:

  `Enabling site food_truck.dev.`  
  `To activate the new configuration, you need to run:`  
  `  systemctl reload apache2`

- Before we restart the server, we'll test the configuration syntax for errors.  To do this, run the command `apachectl configtest`  You can safely ignore any errors that say "Could not reliably determine the server's fully qualified domain name".  As long as you see `Syntax OK`, it is time to restart Apache.

- To restart Apache, run the command `sudo systemctl reload apache2`

## Edit Host file

If this were a live web server and you used domain names that you have registered, you would need to point your domains to the IP of your Apache server so they can be served across the Internet.  Since we are using development domains, we need to edit the hosts file on our computer to point to these domains on our Apache Server.  Find the relevant guide below based on the OS you are running.

### macOS and Linux

- To edit the hosts file on a Mac or Linux machine, run the command `sudo nano /etc/hosts`

- Once the hosts file is open, enter the following two lines into the file replacing the `IP_ADDRESS` with the IP address of your Raspberry Pi web server and the domains with your own domains.

  `IP_ADDRESS jdoe.dev`
  `IP_ADDRESS food_truck.dev`

- Save and close the file by pressing Ctrl+X, type `Y`, and hit Enter.

### Windows 8/10

- Click on the Start windows icon to open the menu and type `Notepad`.

- Right click on __Notepad__ and choose __Run as administrator__. If you are prompted for an administrator password or for a confirmation, type the password of click __Allow__ or __Yes__.

- In Notepad, click __File__ and __Open__ then navigate to the hosts file.  The file is located in `C:\Windows\System32\drivers\etc`.  If the folder appears blank, click on the file extension dropdown where it says __Text Documents (*.txt)__ and change it to __All Files (*.*)__.

- Open the file called __hosts__.

- Once the hosts file is open, enter the following two lines into the file replacing the `IP_ADDRESS` with the IP address of your Raspberry Pi web server and the domains with your own domains.

  `IP_ADDRESS jdoe.dev`
  `IP_ADDRESS food_truck.dev`

- Once you have made the necessary changes, click __File__ and __Save__.

### Windows Vista/7

- Click Start windows icon.  Then, click __All Programs__.  Next, click __Accessories__.  Finally, right-click on __Notepad__ and then click __Run as administrator__. If you are prompted for an administrator password or for a confirmation, type the password of click __Allow__ or __Yes__.

- In Notepad, click __File__ and __Open__ then navigate to the hosts file.  The file is located in `C:\Windows\System32\drivers\etc`.  If the folder appears blank, click on the file extension dropdown where it says __Text Documents (*.txt)__ and change it to __All Files (*.*)__.

- Open the file called __hosts__.

- Once the hosts file is open, enter the following two lines into the file replacing the `IP_ADDRESS` with the IP address of your Raspberry Pi web server and the domains with your own domains.

  `IP_ADDRESS jdoe.dev`
  `IP_ADDRESS food_truck.dev`

- Once you have made the necessary changes, click __File__ and __Save__.
