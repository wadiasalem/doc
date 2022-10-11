# Ubuntu settings to host

## Login & Update

### Login :

With `PuTTY` (https://www.putty.org) we can login to our server using SSH connection. 
Or From using the command line :

      ssh username@IPv4_of_your_VPS

### Updating system

Developers of distributions and operating systems offer frequent software package updates, very often for security reasons. Ensuring that your distribution or operating system is updated is a key point for securing your VPS.

This update will take place in two steps.
First step we need:

* Updating the package list

      sudo apt update

* Updating the actual packages

      sudo apt upgrade

This operation needs to be performed regularly to keep a system up-to-date.

## Changing the default SSH listening port

One of the first things to do on your server is configuring the SSH service’s listening port. It is set to **port 22** by default, therefore server hacking attempts by robots will target this port. Modifying this setting by using a different port is a simple measure to harden your server against automated attacks.

To do this, modify the service configuration file with a text editor of your choice (`nano` used in this example):

      ~$ sudo nano /etc/ssh/sshd_config

You should find the following or similar lines:

      # What ports, IPs and protocols we listen for
      Port 22

Replace the number 22 with the port number of your choice. Please do not enter a port number already used on your system. To be safe, use a number between 49152 and 65535.
Save and exit the configuration file.
### Restart service ssh

Restart the service:

      sudo systemctl restart sshd

This should be sufficient to apply the changes. Alternatively, reboot the VPS (`~$ sudo reboot`).

Remember that you will have to indicate the new port any time you request an SSH connection to your server, for example:

      ssh username@IPv4_of_your_VPS -p NewPortNumber

## Changing user information

### Changing user password

To change the default user password we can user the commande `passwd`

    passwd

and then you can set the new password.

### Changing user username

To manage every aspect of the user database, you use the `usermod` tool.
To change username and user's groupname (it is probably best to do this without being logged in):

To use a temporary user:

* Login with your old credentials and add a new user, e.g. "temporary" in TTY1:

      sudo adduser temporary

* add the user to sudo group

      sudo adduser temporary sudo

* Log out with the command exit.

* Login to the temporary user.

* Rename the username

      sudo usermod -l newUsername oldUsername
      sudo groupmod -n newUsername oldUsername

This however, doesn't rename the home folder.

* To change home-folder, use

      sudo usermod -d /home/newHomeDir -m newUsername

* Delete temporary user and folder:

      sudo deluser temporary
      sudo rm -r /home/temporary