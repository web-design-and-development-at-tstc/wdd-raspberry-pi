## Install Apache on Ubuntu

- It is always a good idea to get in the habit of updating the package manager before installing new software.  Open the terminal and run the command sudo apt-get update and let it finish updating.

- To install Apache on Ubuntu, run the command sudo apt-get install apache2 and type Y to give confirmation to install.

- It will take a few minutes for everything to install.

## Verify Apache Installation

To verify Apache was installed correctly, open up a web browser on your Raspberry Pi and type in the address bar the IP address for your Raspberry Pi.  You should see a page labeled “Apache2 Ubuntu Default Page” as seen in the image below.  

![Default screen for Apache](http://inspiringweb.org/moodle/imed2349/apache_default_page.png)
 
## Basic Apache Service Controls

You may find yourself needing to start, stop, restart, or reload Apache.  Below are the commands to perform each of those actions.

- Stop Apache: sudo systemctl stop apache2.service

- Start Apache: sudo systemctl start apache2.service

- Restart Apache: sudo systemctl restart apache2.service

- Reload Apache: sudo systemctl reload apache2.service

## Publish Web Sites

To publish websites on your Raspberry Pi web server, you will need to place all files and directories inside the Apache document root directory located at /var/www/html.
