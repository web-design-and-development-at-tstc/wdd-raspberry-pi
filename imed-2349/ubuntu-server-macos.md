# Set up your SD Card - macOS Instructions

To set up your SD card, you need a laptop or desktop computer that has an SD card port or a USB card reader.

We are going to be installing Ubuntu Server onto your SD card to be the OS for our Raspberry Pi.

If you are doing this using the Mac’s and the Raspberry Pis in the lab, you will need to have the instructor log in using an admin account.  If you are using your personal computer, there will be software that you will have to install to perform some of these steps.  Those will be detailed out during the instructions.

## Download Ubuntu Server for Raspberry Pi

- Visit the [Install Ubuntu Server on Raspberry Pi 2, 3 or 4 page](https://ubuntu.com/download/iot/raspberry-pi-2-3)

- When running web servers, you don't want to be making major changes to your base OS to frequently, so it's best to pick a version that has long term support (LTS).  Scroll down and under the Raspberry Pi 3 model, click on the button titled “Download 64-bit” for Ubuntu 18.04.4 LTS.  You'll notice that this version of Ubuntu Server will be supported until April of 2023.  If you purchased your own Raspberry Pi and you chose to purchase a different model (for example the Raspberry Pi 4 is the latest model), then you would want to download the image that matches your model.  For the department's Raspberry Pis, we have the model 3 B+, thus we want the image for the Raspberry Pi 3.

- Make sure you download this to your Downloads folder on the Mac.

## Format the microSD Card

**NOTE:** Anything that is stored on the microSD card will be overwritten during the formatting process.  If there are any files/photos that you want to keep, you need to make a backup of these files first so you don’t lose them permanently.

- If you are using your personal computer, visit the SD Association’s website and download [SD Formatter](https://www.sdcard.org/downloads/formatter_4/index.html) for Mac.  Follow the instructions to install the software.

- Insert your SD card into the computer or laptop’s SD card slot or into the SD card slot on the USB card reader and plug the reader into a USB port on your computer.  

- In SD Formatter, select your SD card, and then format that card.  The Mac probably will ask for an administrator username and password before it will format the card.

## Flash the microSD Card

- If you are using a personal computer to flash the microSD card with the image, you will need to install a couple of things on the Mac.

  - The first is to install the Command Line Tools.  To do this, Launch the Terminal then run the command `xcode-select --install`

  - This will launch a software update popup window and you will need to click Install.  Then you will need to accept the license agreement by clicking Agree.  Once it has finished, click Done.

  - Next, you are going to install Homebrew.  Enter the command `/usr/bin/ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”`

  - There will be a series of lines showing what will be installed and where.  Hit Return to agree.  It will ask for the administrator password.  Once this has been entered the installation of Homebrew will begin.

  - Once the installation is finished, you’ll see the message `Installation successful!`

  - After Homebrew is installed, run the command `brew install xz`

- Whether you are using your personal computer or one of our Mac computers, launch the Terminal and run the command `diskutil list`

- Identify the device drive address for your microSD card, for me it is `/dev/disk2`, which I’ll be using throughout the rest of these directions.  You will need to replace this with the device drive address that matches your microSD card.

- Once you have the device drive address, we are going to dismount our microSD card using the command: `diskutil unmountDisk /dev/disk2`

- If successful, you’ll see a message similar to `Unmount of all volumes on disk2` was successful

- Next, we are going to copy the image to the microSD card.  To do this you either have to be logged in as an administrator or be using a user account that has sudo rights.  If you are using an SD card that is not 32GB, then you need to run the command: `sudo sh -c ‘xzcat ~/Downloads/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz | sudo dd of=/dev/disk2 bs=32m’`

- It will ask for the administrator password.  Enter this and hit enter.

- It will take approximately 10 minutes to run and you won’t see any activity in the Terminal window during this time.  It is finished when you see a message similar to:

  `0+36988 records in`  
  `0+36988 records out`  
  `2422361088 bytes transferred in 637.409123 secs (3800324 bytes/sec)`

- Now you can eject your microSD card.

## Boot the Raspberry Pi

- Insert the SD Card you’ve just flashed into the micro SD card slot located on the underside of the Raspberry Pi board.

- Plugin the power cable, an HDMI monitor, a mouse, a keyboard, and an Ethernet cable.

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

- Next, we are going to set it, so our date and time are synchronized as opposed to having to manually set it in the future.  To do this, run the command `sudo apt install chrony`.  When it asks to confirm, type `Y` and hit Enter.

- After it has installed, run the command `systemctl status chronyd`  You should see output that shows that it is active.

- To enable chrony to run upon boot, run the command `systemctl enable chrony`.

- You will get the message `==== AUTHENTICATING FOR org.freedesktop.systemd1.reload-daemon ===` and that authentication is required to reload the systemd state.  The command line will ask for your password.  Enter your password (keeping in mind you won't see any characters on screen) and hit enter.  You'll get the message `==== AUTHENTICATION COMPLETE ===`.

- You will get a second message `==== AUTHENTICATING FOR org.freedesktop.systemd1.reload-daemon ===` and that authentication is required to reload the systemd state.  The command line will ask for your password.  Enter your password (keeping in mind you won't see any characters on screen) and hit enter.  You'll get the message `==== AUTHENTICATION COMPLETE ===`.

- Next, you will get the message `==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-unit-files ===` and that authentication is required to reload the systemd state.  The command line will ask for your password.  Enter your password (keeping in mind you won't see any characters on screen) and hit enter.  You'll get the message `==== AUTHENTICATION COMPLETE ===`.

- You will get a final message `==== AUTHENTICATING FOR org.freedesktop.systemd1.reload-daemon ===` and that authentication is required to reload the systemd state.  The command line will ask for your password.  Enter your password (keeping in mind you won't see any characters on screen) and hit enter.  You'll get the message `==== AUTHENTICATION COMPLETE ===`.

- You can verify the system date and time by running the command `timedatectl`

## Change Hostname

By default, all new Ubuntu Server installations have the same hostname, ubuntu.  To differentiate them on the network, we need to change the hostname.

- Run the command `hostnamectl set-hostname raspberrypi-WDD-jdoe` (where j is the initial of your first name and doe is your last name; so for example my hostname would be `raspberrypi-WDD-dtrower`).  This step requires authentication.  Enter your password for the Ubuntu user and hit enter.  You should see the message `==== AUTHENTICATION COMPLETE ====`.

## Get IP Address

Next, we need to get the IP address for our Raspberry Pi so we can remote into the Raspberry Pi.

- To find the IP address for your Raspberry Pi, type the command `ifconfig` and hit Enter.

- This will print a bunch of information to the command line.  To identify your IP address, look for the inet address under eth0.  Make sure you write this IP address down because you will need it to connect to the Raspberry Pi from another computer.

## Verify SSH Status

In this class, after the initial setup, we are going to remote into our Raspberry Pi.  You will only be able to remote into the Raspberry Pi while you are on the same network as the Raspberry Pi.

- To verify that the SSH server is running, run the command `sudo systemctl status ssh`.  You should see in the output the message `Active: active (running)`.

## Connect to Raspberry Pi using SSH

For the rest of the installation and setup process, you need to remote into your Raspberry Pi using an SSH client.  An SSH client is built into the macOS, so all you need to do is open up the Terminal window.  You do this by going to `Finder -> Applications -> Utilities -> Terminal`.

- In the Terminal window, run the command `ssh ubuntu@IP_ADDRESS` where you will replace the words IP_ADDRESS with the IP address you wrote down earlier and hit Enter.

- The first time you try to connect to your Raspberry Pi remotely via the Terminal, you will get a message that the authenticity of host IP_ADDRESS can't be established and a ECDSA key fingerprint.  It will ask you if you want to continue connecting.  Type in `Yes` and hit Enter.

- This will generate a warning that the IP_ADDRESS (ECSDA) has been permanently added to the list of known hosts and then will ask for your password.  Enter the password for the Ubuntu user on the Raspberry Pi (remember you won't be able to see any characters on screen as you enter the password).

- You know you have been successfully connected to the remote Ubuntu Server When you see the message "Welcome to Ubuntu 19.10" along with additional information.  Your other visual clue that you are successfully connected is you Terminal window now shows the user as ubuntu@raspberrypi-WDD-jdoe (ubuntu@HOSTNAME) instead of username@name_of_computer.  The terminal also changes the color of a remote user to lime green whereas the local user is in white.  Now anything you do in the Terminal window until we exit will be performed on the Ubuntu Server running on the Raspberry Pi instead of on the Mac computer.

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

- We’ll start by creating the file to be used for swap by running the command `sudo fallocate -l 4G /swapfile` (the argument on this command is the lowercase "L" NOT the number "1").

- Next, we want to set it so only the root user can read and write to the file, so we are going to set the correct permissions by running the command `sudo chmod 600 /swapfile`

- Then we need to set up a Linux swap area on the file by running the command `sudo mkswap /swapfile`.  You should get results that look similar to:

  `Setting up swapspace version 1, size = 4 GiB (4294963200 bytes)`  
  `no label, UUID=de269b7b-93fe-4c99-810c-4f8aa85ba9cb`

- Next, we’ll activate the swap file by running the command `sudo swapon /swapfile`

- To make the swap available permanently, we are going to open a file called /etc/fstab and add a line to the file.  To do this, run the command `sudo nano /etc/fstab`

- Once the file is open in the command line, at the end of the file add the line of text `/swapfile swap swap defaults 0 0`

- Then hit Control + X, then hit `Y`, and then hit Enter.  This saves the file.

- To verify that your swap is now active, run the command `sudo free -h`

- You should see something similar to the following (don’t worry if the values in the total column don’t match yours):

  `              total        used        free      shared  buff/cache   available`  
  `Mem:          906Mi       193Mi       194Mi        3.0M        518M        688M`  
  `Swap:         4.0Gi           0B       4.0Gi                                    `

## Configure Firewall

For added security, it is recommended that you configure a firewall on your server.

- Before we enable it, we are going to add a rule for SSH since we are configuring our server remotely, we don't want to get locked out when we enable the firewall.  Run the command `sudo ufw allow OpenSSH`.  You should get the output:

  `Rules updated`  
  `Rules updated (v6)`

- If you got the message that rules updated, then enable the firewall.  To do this, run the command: `sudo ufw enable`.

- The Terminal will display the message "Command may disrupt existing ssh connections. Proceed with operation (y|n)?"  Type `Y` and hit Enter.

- You should get the message, "Firewall is active and enabled on system startup".

- You can check the current status of your firewall by running the command `sudo ufw status`.
