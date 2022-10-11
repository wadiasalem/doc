# Ubuntu settings to host

## Login & Update

### Login :

With `PuTTY` (https://www.putty.org) we can login to our server using SSH connection. 

### Updating system

Developers of distributions and operating systems offer frequent software package updates, very often for security reasons. Ensuring that your distribution or operating system is updated is a key point for securing your VPS.

This update will take place in two steps.
First step we need:

* Updating the package list

      sudo apt update

* Updating the actual packages

      sudo apt upgrade

This operation needs to be performed regularly to keep a system up-to-date.
