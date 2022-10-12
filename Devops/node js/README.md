# Install Node in your Ubuntu

## Download node

you can specify the node version from changing ***16.x*** `https://deb.nodesource.com/setup_16.x`.
in this example we will install the 16 version:

      curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -

## Install

To install the downloaded node:

      sudo apt-get install -y nodejs

## config pm2

### install

To run a server we will use the pm2 package

      npm i pm2 -g

### Save pm2

To freez pm2 status we need to login as a root then :

      pm2 startup systemd -u user_name --hp /home/user_directory
      pm2 save

## Start server

Using pm2 we can start the server giving him a name:

      sudo pm2 start --name server server.js

## Stop server

to stop a server by his name :

      sudo pm2 stop server