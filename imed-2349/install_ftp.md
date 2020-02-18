## Install FTP Server

We are going to use the popular FTP server vsftpd (Very Secure File Transfer Protocol Daemon) for Ubuntu.

- To begin, we are going to update the package lists by running the command `sudo apt update`

- Next, we'll download and install vsftpd by running the command `sudo apt install vsftpd`.  When it asks if you want to continue, type `Y` and hit Enter.

- Once it has installed, we can check the status to make sure vsftpd is running by running the command `sudo service vsftpd status`.  You should see a message similar to the one below if it is running correctly:

  `● vsftpd.service - vsftpd FTP server`  
     `Loaded: loaded (/lib/systemd/system/vsftpd.service; enabled; vendor preset: enabled)`  
     `Active: active (running) since Mon 2020-02-17 17:14:26 CST; 31s ago`  
   `Main PID: 6924 (vsftpd)`  
  `    Tasks: 1 (limit: 4440)`  
  `   CGroup: /system.slice/vsftpd.service`  
  `           └─6924 /usr/sbin/vsftpd /etc/vsftpd.conf`  

  `Feb 17 17:14:26 raspberrypi-WDD-dtrower systemd[1]: Starting vsftpd FTP server...`  
  `Feb 17 17:14:26 raspberrypi-WDD-dtrower systemd[1]: Started vsftpd FTP server.`

## Configure Firewall

Since we enabled the ufw firewall, we need to add some additional rules to open up the ports that are needed for FTP, ports 20 and 21 for FTP and ports 40000-50000 for passive FTP.  We'll also open up port 990 for TLS.

- First, run the command `sudo ufw allow 20/tcp`.  You should get the output:  
  `Rules updated`  
  `Rules updated (v6)`  

- Next, run the command `sudo ufw allow 21/tcp`.  You should get the output:  
  `Rules updated`  
  `Rules updated (v6)`

- Then, run the command `sudo ufw allow 40000:50000/tcp`.  You should get the output:  
  `Rules updated`  
  `Rules updated (v6)`

- Finally, run the command `sudo ufw allow 990/tcp`.  You should get the output:  
  `Rules updated`  
  `Rules updated (v6)`

- You can check the current status of your firewall by running the command `sudo ufw status`.  Provided you have been adding the rules as we have gone along, you should get the following status and list of active rules:  
  `Status: active`  

  `To                         Action      From`  
  `--                         ------      ----`  
  `OpenSSH                    ALLOW       Anywhere`  
  `Apache Full                ALLOW       Anywhere`  
  `20/tcp                     ALLOW       Anywhere`  
  `21/tcp                     ALLOW       Anywhere`  
  `40000:50000/tcp            ALLOW       Anywhere`  
  `990/tcp                    ALLOW       Anywhere`  
  `OpenSSH (v6)               ALLOW       Anywhere (v6)`  
  `Apache Full (v6)           ALLOW       Anywhere (v6)`  
  `20/tcp (v6)                ALLOW       Anywhere (v6)`  
  `21/tcp (v6)                ALLOW       Anywhere (v6)`  
  `40000:50000/tcp (v6)       ALLOW       Anywhere (v6)`  
  `990/tcp (v6)               ALLOW       Anywhere (v6)`

## Create FTP User

To be able to log in to FTP, we need to create a user.

- To create the user, `ftpuser` run the command `sudo addsuer ftpuser`

- It will ask you to enter new UNIX password.  For our purposes, let's use the password `WDDrules20`.  Like with other passwords, you won't see any characters on screen.  Once you have typed in the password, hit Enter.

- It will then ask you to retype the password.  After you have retyped it, hit Enter.

- Next, it will ask for some contact information (full name, room number, work phone, home phone, and other). You can leave these blank and just hit Enter for each value.

- After the last value (other), it will ask `Is the information correct? [Y/n]`, type `Y` and hit Enter.

## Directory Permissions

Since we are going to be using our FTP server to transfer files to a web server, we'll set the home folder for the `ftpuser` to `/var/www/`.  This will allow the user to be able to upload to any site we host on our web server.  In this same way, if we were hosting multiple client websites and wanted to give each client access to just their website via FTP, we can follow these same steps and use a more specific home folder that limits their access to just their website.

- To set the folder above the document root as the home directory for the `ftpuser`, run the command `sudo usermod -d /var/www ftpuser`

- Next, we'll set the ownership of the document root directory to `ftpuser`.  This will allow our FTP user to  write and alter files in the document root directory.  To do this, run the command `sudo chown ftpuser:ftpuser /var/www/html`

## Configure vsftpd

Before we can start using FTP on our Ubuntu Server, there are a few changes we need to make to the vsftpd configuration file.

- Before we edit the file, let's create a backup.  Run the command `sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak`

- Next, open the configuration file in nano by running the command `sudo nano /etc/vsftpd.conf`

- First, we'll allow FTP users to write files to the server. Press Ctrl+W and type `#write_enable`.  Then press Enter.  This will go to the place in the file where this directive is set.  Remove the pound sign (`#`) from the start of the line to uncomment the directive and make sure the directive is set to `YES`.

- Next, we'll limit the FTP users from browsing outside their own directory.  Press Ctrl+W and type `#chroot_local_user`.  Then press Enter.  This will go to the place in the file where this directive is set.  Remove the pound sign (`#`) from the start of the line to uncomment the directive and make sure the directive is set to `YES`.

- Then, we'll make sure uploaded files and folders are given the correct permissions. Press Ctrl+W and type `#local_umask`.  Then press Enter.  This will go to the place in the file where this directive is set.  Remove the pound sign (`#`) from the start of the file to uncomment the directive and make sure the directive is set to `022`.

The next two directives don't exist in the file so we are going to have to add them.  The first directive will force vsftpd to show file names that being with a dot, like `.htaccess`.  By default, Linux doesn't show these files and thus they won't be visible in FTP which is a problem if you intend to use Apache and work with `.htaccess`.

- To force vsftpd to show file names that begin with a dot, move all the way to the bottom of the file and type in the directive `force_dot_files=YES`

- Finally, we'll want to add some port ranges for passive FTP to make sure enough connections are available. Add these directive to the end of the file:  
  - `pasv_min_port=40000`  
  - `pasv_max_port=50000`

- To save the file and exit nano, press Ctrl+X, then type `Y`, and hit Enter.

- For these configuration changes to take effect, we need to restart the FTP server.  Run the command `sudo systemctl restart vsftpd`

## Test FTP

To make sure that our FTP server is working correctly, we are going to test vsftpd to see if we can log in with the user we created earlier.

- Open up FileZilla and in the Quickconnect bar, enter your server's IP address for the Host, `ftpuser` for the username, and, if you used the password specified, `WDDrules20` for the password.  Then click __Quickconnect__.

- You'll get a dialog box that says "This server does not support FTP over TLS.  If you continue, your password and files will be sent in clear over the internet."  For now, click __OK__.  In the next section, we'll set up TLS on our server.

- On the right hand side, you'll see that the only folder listed is the html folder which is located inside `/var/www`.  If you open this folder, you'll see an index.html file and the info.php file we created during the PHP installation.  Remember, anything you upload here will be live on your webserver.  Try uploading, creating, and editing folders and files within the web root directory (`/var/www/html`) to ensure permissions are working correctly.

- Once you have finished testing, close your FileZilla.

## Secure FTP with TLS

If you recall when you first logged into FileZilla, you received a dialog box warning you that the server didn't support FTP over TLS and that your password and files would be sent in the clear over the Internet.  By default, when using FTP, your connection and anything you transfer is not encrypted.  To address this, you should connect to FTP using FTPS (FTP over SSL/TLS).

- To begin, we need to create a new certificate using the `openssl` tool.  Run the command `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem`

- You will be asked to enter some details (like country).  You can either enter the information or just hit enter to leave it at the default value.

- Now that we have created our private key, we need to make some additional changes to our vsftpd configuration file.  Open the file by running the command `sudo nano /etc/vsftpd.conf`

-  First, we'll want to enable SSL. Press Ctrl+W and type `ssl_enable`.  Then press Enter.  This will go to the place in the file where this directive is set.  Set the variable to `YES` instead of the default `NO`.

- Beneath that variable add the following directives:  
  `rsa_cert_file=/etc/ssl/private/vsftpd.pem`
  `rsa_private_key_file=/etc/ssl/private/vsftpd.pem`
  `allow_anon_ssl=NO`
  `force_local_data_ssl=YES`
  `force_local_logins_ssl=YES`
  `ssl_tlsv1=YES`
  `ssl_sslv2=NO`
  `ssl_sslv3=NO`
  `require_ssl_reuse=NO`
  `ssl_ciphers=HIGH`

- Save the file and exit by pressing Ctrl+X, then type `Y`, and hit Enter.

- Finally, for these changes to take effect, we need to restart vsftpd.  To do this, run the command `sudo systemctl restart vsftpd`

## Testing FTP with TLS

To make sure that our FTP server now utilizes FTP over SSL/TLS, we are going to log in with the user we created earlier.

- Open up FileZilla and in the Quickconnect bar, enter your server's IP address for the Host, `ftpuser` for the username, and, if you used the password specified, `WDDrules20` for the password.  Then click __Quickconnect__.

- You should see a dialog box similar to the one below saying the server's certificate is unknown.  Near the bottom, there is a checkbox to "Always trust certificate in future sessions."  Go ahead and check this checkbox and then click __OK__.

![Example Dialog Box for Unknown Certificate in FileZilla](http://inspiringweb.org/moodle/imed2349/ftp_tls_unknown_certificate.png)

- Try uploading, creating, and editing folders and files within the web root directory (`/var/www/html`) to ensure everything is still working correctly.

- Once you have finished testing, close your FileZilla.
