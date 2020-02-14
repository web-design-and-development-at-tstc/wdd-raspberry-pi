## Setting Up Virtual Hosts

With Apache, it is possible to host multiple sites/domains on Apache using Virtual Hosts.  Even if you are only going to host a single site/domain, it is still a good idea to set up a directory and Virtual Host. If you ever decide to add another domain later, things will be a lot easier for you.  For this guide, we are not going to use registered domains and we'll set up the hosts file in our OS to resolve the domains we use in this guide.

The first domain we are going to make a virtual host for is `jdoe.dev` (where j is the initial of your first name and doe is your last name; so for example my first domain would be `dtrower.dev`).  The second domain we are going to make a virtual host for is a development domain for your food truck/restaurant.  In this guide, replace `jdoe.dev` with the domain using your name and replace `food_truck.dev` with a development domain for your food truck/restaurant.

## Create Directories and Set Permissions

Inside the `/var/www/` directory we are going to create two new directories for our two domains.  Remember to replace `jdoe.dev` and `food_truck.dev` with your own development domains.

- Run the command `sudo mkdir -p /var/www/jdoe.dev/public_html`.

- Then run the command `sudo mkdir -p /var/www/food_truck.dev/public_html`.

- Next we'll need to change the permissions of the general web directory `/var/www` and its contents so our websites can be served correctly.  To do this, run the command, `sudo chmod -R 755 /var/www`.

## Create Test Web Pages

For each domain we are going to create a simple index.html web page to test that our setup is correct.  Remember to replace `jdoe.dev` and `food_truck.dev` with your own development domains.

- To create the first index.html web page for `jdoe.dev`, run the command `sudo nano /var/www/jdoe.dev/public_html/index.html`.

- In the nano editor, Create a simple HTML web page with the your name in the &lt;title&gt; element and inside an &lt;h1&gt; element so we can see something in the tab and the browser window when we load the website.

- To exit the nano editor and save this file, press CTRL+X to exit, type `Y` to save, and Enter to confirm the file name.

- We'll do the same thing for food_truck.dev.  Run the command `sudo nano /var/www/food_truck.dev/public_html/index.html`.

- In the nano editor, Create a simple HTML web page with the name of your food truck/restaurant in the &lt;title&gt; element and inside an &lt;h1&gt; element so we can see something in the tab and the browser window when we load the website.

- To exit the nano editor and save this file, press CTRL+X to exit, type `Y` to save, and Enter to confirm the file name.

## Create New Virtual Host Files

The Virtual Host files are used to tell Apache how to respond to various domain requests.

- To begin, we are going to create a new virtual host file for our `jdoe.dev` domain.
