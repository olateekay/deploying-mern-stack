# DEPLOYING A MERN STACK
**Prerequisites**

We need access to an Ubuntu 20.04 server

*Step 1 – Backend configuration*

Update ubuntu

`sudo apt update`

Upgrade ubuntu

`sudo apt upgrade`

![ubuntu upgrade and update](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern1.JPG)

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern2.JPG)

Get the location of Node.js software from Ubuntu repositories.

`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern3.JPG)

*Install Node.js on the server* 

Install Node.js with the command

`sudo apt-get install -y nodejs`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern4.JPG)


Note: The command above installs both nodejs and npm.

Verify the node and npm installation with the command below

`node -v `

`npm -v`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern5.JPG)


*Application Code Setup*

Create a new directory for your To-Do project:

`$ mkdir Todo`

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern6.JPG)


verify that the `Todo` directory is created with 

`$ ls`


Now change your current directory to the newly created one:

`$ cd Todo`

Next, you will use the command `npm init` to initialise your project, so that a new file named `package.json` will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press `Enter` several times to accept default values, then accept to write out the `package.json` file by typing yes.

![alt text](https://github.com/olateekay/deploying-mern-stack/blob/main/images/mern7.JPG)
