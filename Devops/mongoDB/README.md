# Using mongoDB in ubuntu
## Download and install
From a terminal, issue the following command to import the MongoDB public GPG Key.

      wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -

The operation should respond with an `OK`.

      echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

### Reload local package database.

Issue the following command to reload the local package database:

       sudo apt-get update

### Install the MongoDB packages

To install the latest stable version, issue the following

      sudo apt-get install -y mongodb-org

## Stqrt mongo

You can start the mongod process by issuing the following command:

    sudo systemctl start mongod

### Verify that MongoDB has started successfully.

      sudo systemctl status mongod

You can optionally ensure that MongoDB will start following a system reboot by issuing the following command:

      sudo systemctl enable mongod

