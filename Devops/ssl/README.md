# Generate ssl

## Installing Snapd

Snap is a package manager developed by Canonical (creators of Ubuntu). Software is packaged as a snap (self-contained application and dependencies) and the snapd tool is used to manage these packages. Since certbot is packaged as a snap, we’ll need to install snapd before installing certbot. While it’s installed by default on Ubuntu 16.04 and later, its also available for most other Linux distributions.

### If snapd is not installed, install it now.

      sudo apt update
      sudo apt install snapd

### Install the core snap.

      sudo snap install core
      sudo snap refresh core

## Installing Certbot

The next step is to install Certbot using the snap command.

Remove any previously installed certbot packages to avoid conflicts with the new Snap package.

      sudo apt remove certbot

Use Snap to install Certbot.

      sudo snap install --classic certbot

Configure a symbolic link to the Certbot directory using the ln command.

      sudo ln -s /snap/bin/certbot /usr/bin/certbot

## Requesting a TLS/SSL Certificate Using Certbot

During the certificate granting process, Certbot asks a series of questions about the domain so it can properly request the certificate. You must agree to the terms of service and provide a valid administrative email address. Depending upon the server configuration, the messages displayed by Certbot might differ somewhat from what is shown here.

### Request a certfifcate and automatically configure it on NGINX (recommended):

      sudo certbot --nginx

### Request a certificate without configuring NGINX:

      sudo certbot certonly --nginx