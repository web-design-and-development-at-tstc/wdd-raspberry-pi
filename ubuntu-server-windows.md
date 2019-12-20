# Set up your SD Card - Windows Instructions

To set up your SD card, you need a laptop or desktop computer that has an SD card port or a USB card reader.  These instructions were written using Windows 10.  If you are using a different version of Windows, then the steps might be a little different.

We are going to be installing Ubuntu Server onto your SD card to be the OS for our Raspberry Pi.

## Download Ubuntu Server for Raspberry Pi

- Visit the [Install Ubuntu Server on Raspberry Pi 2, 3 or 4 page](https://ubuntu.com/download/iot/raspberry-pi-2-3) 

- Scroll down and click on the button titled “64-bit for Raspberry Pi 3 and 4”.

- Make sure you download this to your Downloads folder on the Mac.

## Format the microSD Card

**NOTE:** Anything that is stored on the microSD card will be overwritten during the formatting process.  If there are any files/photos that you want to keep, you need to make a backup of these files first so you don’t lose them permanently.

- Visit the SD Association’s website and download [SD Formatter](https://www.sdcard.org/downloads/formatter/index.html) for Windows.  Follow the instructions to install the software.  Depending on the version of Windows you are running, you’ll probably have several User Account Control messages appear during the installation process and when you go to open the application.  You will need to click Yes giving the app permission to make changes to your device.

- Insert your SD card into the computer or laptop’s SD card slot or into the SD card slot on the USB card reader and plug the reader into a USB port on your computer.  

- In SD Formatter, select your SD card, and then format that card.

## Flash the microSD Card

- Before you can flash the microSD card, you will need to install a couple of things on your Windows computer.

  - The first is to download and install [balenaEtcher](https://www.balena.io/etcher/).  Follow the instructions to install the software.

  - Next, you need to extract the Ubuntu Server image from the .xz file you downloaded.  To do this you need an archive extractor like [7-zip](https://www.7-zip.org/download.html) or [WinRAR](https://rarlab.com/download.htm).  If you don’t have one of these installed, then you’ll need to download and install one of them.

  - Once you have an archive extractor installed, you need to extract the Ubuntu Server image making sure to keep note of where the image is being extracted to.

- Once you have installed balenaEtcher and extracted the Ubuntu Server image, you are now ready to flash the microSD card.  Open up balenaEtcher.

- Click the button Select image.  Navigate to where you extracted the Ubuntu Server image and select it.  The file should end with the extension .img.

- Next, make sure that the drive that is selected is the microSD card that you formatted earlier.

- Once the image and the drive are selected, click the button labeled Flash!  If you get a User Access Control window, click Yes. 

- It will take approximately 2 minutes to flash the microSD card.  You’ll see a progress bar showing the percentage flashed and an ETA.  Then it will validate the flash.  Again, you’ll see a progress bar and an ETA.  If during the flashing or validation process Windows asked if you want to format a disk in a drive, click Cancel.

- Now you can eject your microSD card.

## Boot the Raspberry Pi

- Insert the SD Card you’ve just flashed into the micro SD card slot located on the underside of the Raspberry Pi board.

- Plugin the power cable, an HDMI monitor, a mouse, a keyboard, and for the initial setup, an Ethernet cable.

- Flick the switch located on the power cable attached to the Raspberry Pi and this will turn the Raspberry Pi on.

- This will load the Ubuntu Server and initialize it.  If there are any prompts that appear, make sure to follow them. Once you see messages related to the SSH Key, and no more activity is occurring on screen, hit Return.

- It will ask for the ubuntu login.  The default username is `ubuntu` and the default password is `ubuntu`.

- It will prompt you to immediately change the password for the root user account.  You will first enter the current password, `ubuntu` and hit Enter.

- Then it will ask for the new password.  I would use the password we are using in class `WDDrules19`.  Normally, you would want to use a password no one else knows or can guess.

- After you have entered the password (you won’t see any characters on screen), hit Enter.

- It will ask you to confirm the new password by having you enter it again (you won’t see any characters on screen).

- After you have entered the password a second time, hit Enter.

- After you have changed the password, you will see a welcome message with some various information.

## Set Date and Time

Before moving on with updating and installing new packages, we need to make sure that the date is correct.

- To see whether the date is correct or not, run the command `date` If the date and time are not correct, we will need to run a few commands.

- First, we are going to set the timezone.  To verify the exact timezone we want to enter, we are going to run a command to display a list of timezones.  Run the command `timedatectl list-timezones`

- You will need to hit enter until you see a timezone like `America/Chicago` or another city located in the Central timezone.  Once you verify the timezone name, make a note and then type `q` and hit enter.

- To set the timezone, we are going to run the command `sudo timedatectl set-timezone America/Chicago`

- Then we are going to run the command to set the date and time.  Run the command `sudo date -s “29 October 2019 08:48:00”` making sure to use the current date and time (in 24-hour military time) ensuring that you match the format shown.

- To verify that the date and time have been changed, run the command date

## Update Firmware

- To update the firmware on the Raspberry Pi as well as any installed software, we will enter several commands in our Command Line.

- Type `sudo apt-get update` into the Command Line and hit Enter

- Once it has finished running, type `sudo apt-get upgrade -y` and hit Enter.

- After it has run, type `sudo apt-get dist-upgrade -y` and hit Enter.

- Next, we are going to set it, so our date and time are synchronized as opposed to having to manually set it in the future.  To do this, run the command `sudo apt install chrony` When it is finished, it will tell you how much additional space will be taken up by the install and will ask if you want to continue.  Type `Y` and hit Enter.

- After it has installed, run the command `systemctl status chronyd`  You should see output that shows that it is active. 

- To enable chrony to run upon boot, run the command `systemctl enable chrony`

- You can verify the system date and time by running the command `timedatectl`

- After all of these have been run, you will need to reboot the Ubuntu Server.  Type the comman `sudo shutdown –r now` to initiate the reboot.

- Once it has come up, hit Enter and then enter the username, `ubuntu`, and hit Enter.  Then enter your password and hit Enter.

## Change Hostname

By default, all new Ubuntu Server installations have the same hostname, ubuntu.  To differentiate them on the network, we need to change the hostname.

- Run the command `hostnamectl set-hostname raspberrypi-WDD-jdoe` (where j is the initial of your first name and doe is your last name; so for example my hostname would be `raspberrypi-WDD-dtrower`).  This step requires authentication.  Enter your password for the Ubuntu user and hit enter.  You should see the message `==== AUTHENTICATION COMPLETE ====`.

## Install Fail2Ban

Fail2Ban blocks suspicious requests that come from the Internet.  It will block the IP address if there are too many attempts to guess the password.

- In the Terminal window, type `sudo apt-get install fail2ban`

- It will download everything needed and then you will be asked if you want to continue.  Type `Y` and hit Enter.

- After it has finished running, type `sudo systemctl enable fail2ban` and hit Enter.

- After it has finished running, we need to set some settings for this.  To begin we are going to create a new file.  Type the command `sudo nano /etc/fail2ban/jail.local` and hit Enter.

- In the editor, you will need to enter the following information:

  `[DEFAULT]`  
  `# Ban hosts for one hour:`  
  `bantime = 3600`  
  
  `# Override /etc/fail2ban/jail.d/00-firewalld.conf:`  
  `banaction = iptables-multiport`  
  
  `[sshd]`  
  `enabled = true`

- To exit the nano editor and save this file, press CTRL+X to exit, type `Y` to save, and Enter to confirm the file name.

- For these settings to take effect, we need to restart the fail2ban service.  To do this, type the command `sudo systemctl restart fail2ban` and hit Enter.

- To verify that the service is running, we can use the fail2ban-client.  Type the command `sudo fail2ban-client status` and hit Enter.

- You should see the output:

  `Status`  
  `|- Number of jail:      1`  
  `` `- Jail list:   sshd``

## Create a Swap File

Swap is space on a disk that the OS can use when the amount of physical RAM memory is full.  To create a swap file, follow these steps.

- Verify that you don’t already have a swap enabled by running the command `sudo swapon –show`

- If there is no output, then you don’t have any swap space enabled and should proceed with the following steps.

- We’ll start by creating the file to be used for swap by running the command `sudo fallocate -l 2G /swapfile`

- Next, we want to set it so only the root user can read and write to the file, so we are going to set the correct permissions by running the command `sudo chmod 600 /swapfile`

- Then we need to set up a Linux swap area on the file by running the command `sudo mkswap /swapfile`

- Next, we’ll activate the swap file by running the command `sudo swapon /swapfile`

- To make the swap available permanently, we are going to open a file called /etc/fstab and add a line to the file.  To do this, run the command `sudo nano /etc/fstab`

- Once the file is open in the command line, at the end of the file add the line of text `/swapfile swap swap defaults 0 0`

- Then hit Control + X, then hit `Y`, and then hit Enter.  This saves the file.

- To verify that your swap is now active, run the command `sudo free -h`

- You should see something similar to the following (don’t worry if the values in the total column don’t match yours):

  `              total        used        free      shared  buff/cache   available`  
  `Mem:           488M        158M         83M        2.3M        246M        217M`  
  `Swap:          2.0G        506M        517M`

## Install an Ubuntu GUI Desktop

By default, the Ubuntu Server is a command line only interface.  However, we can install a GUI desktop to use.  

- To do this, from the Command Line on Ubuntu run the following command: `sudo apt-get install xubuntu-desktop`

- You will see a wall of text scroll by on the screen.  When it is finished, it will tell you how much additional space will be taken up by the install and will ask if you want to continue.  Type `Y` and hit Enter.

- This is going to take an extremely long time to run. Early on in the installation process there may be a couple of prompts that you have to respond to, but for the most part, this process just needs time to run.  I would work on other work for this class, while it is running.

- Once it has finished installing, you will need to reboot the Ubuntu Server by running the command `sudo shutdown –r now`

- When Ubuntu Server finished rebooting, it will bootup to the login screen for the Xubuntu GUI desktop.  Enter your password and then hit Enter.
