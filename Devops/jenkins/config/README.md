# jenkins setup for ubuntu

## Install JDK

To use jenkins we need to install `jdk`

      sudo apt install openjdk-11-jre

## Download jenkins

      curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
        /usr/share/keyrings/jenkins-keyring.asc > /dev/null

    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null

## Update the syste

      sudo apt-get update

## Install jenkins

We can now install the downloaded jenkins:

      sudo apt-get install jenkins

## Enable jenkins

After installation jenkins need to be enabled:

      sudo systemctl enable jenkins

## jenkins start

jenkins can start running by using 

      sudo systemctl start jenkins

## jenkins status

To see the jenkins Status if it is running we can use:

      sudo systemctl status jenkins

if every think is good you can see `active (running)` as a status.

## start using jenkins

jenkins use the `Port 8080`, we can navigate to it by using `server_ip:8080`, this will give you the starter interface that require
a password to verify admin use.

Jinkis has write the inisial password in his log, to find it we need:

      journalctl -u jenkins.service

Now we can install our need of packages and we create the admin user.

## jenkins access

In this step jenkins is installing and running good the last think we need to do is to give jenkins permission to the user directory
and sudo group.
To do this we need to switch root user.

      su -

      sudo chown -R username:jenkins /home/user_directory/

* `username` is the username of the user we are using wher we will place our projectes.
* `user_directory` is the home directory of the user.

To give jenkins sudo group:

      visudo -f /etc/sudoers

and add `jenkins ALL= NOPASSWD: ALL` in the end of file.

