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

## To generate ssh key

      ssh-keygen -o

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

## Changing user credentials

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

* Return to our user with new credentials

* Delete temporary user and folder:

      sudo deluser temporary
      sudo rm -r /home/temporary

## Changing the password associated with the user “root”

It is strongly recommended that you modify the password of the root user as to not leave it at default value on a new system.

      sudo su -

## Disabling server access via the root user

The root user is created by default on GNU/Linux systems. Root access means having the highest level of permissions on an operating system. It is not advisable and even dangerous to leave your VPS accessible only via root, as this account can perform irreversibly damaging operations.

We recommend that you disable direct root user access via the SSH protocol. Remember to create another user before following the steps below.

You need to modify the SSH configuration file in the same way as described above:

      sudo nano /etc/ssh/sshd_config

Locate the following section:

      # Authentication: 
      LoginGraceTime 120
      PermitRootLogin yes 
      StrictModes yes

Replace yes with no on the line PermitRootLogin.

For this modification to be taken into account, you need to restart the SSH service:

      sudo systemctl restart sshd

Afterwards, connections to your server via root user (ssh root@IPv4_of_your_VPS) will be rejected.

## For more security Installing Fail2ban

Fail2ban is an intrusion prevention software framework designed to block IP addresses from which bots or attackers try to penetrate your system. This software package is recommended, even essential in some cases, to guard your server against “Brute Force” or “Denial of Service” attacks.

To install the software package, use the following command:

      sudo apt install fail2ban

You can customise the Fail2ban configuration files to protect services that are exposed to the public Internet from repeated login attempts.

As recommended by Fail2ban, create a local configuration file for your services by copying the “jail” file:

      sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

Then open the file with a text editor:

      sudo nano /etc/fail2ban/jail.local
      
Be certain to read the information at the top of the file, especially the comments under [DEFAULT].

The [DEFAULT] settings are global and will therefore be applied to all services that are set to enabled in this file.

It is important to know that the global settings will be taken into account only if there are no differing values set in the services sections (JAILS) further below in the file.

For example, consider these lines under [DEFAULT]:

      bantime  = 10m
      maxretry = 5
      enabled = false

This means that an IP address from which a host tries to connect will be blocked for ten minutes after the fifth unsuccessful login attempt.
However, all settings specified by [DEFAULT] and in subsequent sections stay disabled unless the line enabled = true is added for a service (listed below # JAILS).

As an example of usage, having the following lines in the section [sshd] will activate restrictions only for the OpenSSH service:

      [sshd]
      enabled = true
      port = ssh
      filter = sshd
      maxretry = 3
      findtime = 5m
      bantime  = 30m

In this example, any SSH login attempt that fails three times within five minutes will result in an IP ban period of 30 minutes.

You can replace “ssh” with the actual port number in case you have changed it.

The best practice approach is to enable Fail2ban only for the services that are actually running on the server. Each customised setting added under # JAILS will then be prioritised over the defaults.

Once you have completed your changes, save the file and close the editor.

Restart the service to make sure it runs with the customisations applied:

      sudo service fail2ban restart
