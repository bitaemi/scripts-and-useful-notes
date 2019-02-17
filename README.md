# Setup instructions for a Docker development environment on Windows 10


If Docker is installed open Docker settings and make sure that in “Shared Drivers” section you have the partition where you put your source code checked.

If you do not have  Docker installed on your system, use: How to install Docker on Windows and follow the installation steps.

Setup Project
The following steps are part of updated setup instruction for Docker VM environment installed on Windows 10. On initial setup, all steps need to be performed. Steps 3-8 and 11 need to be performed when you want to start the working.

1.	Clone the project source files from git clone ssh://...git into a project dedicated directory:
git clone ssh://....git
2.	Start Docker machine from Docker’s UI (check in Windows the right arrow icon of the taskbar)

3.	Open cmder program and change directory to the location where you have cloned the project (step 2).

4.	Login your docker image repository using your esp credentials:

docker login ...hub....com
#Username: Enter your windows username
#Password: your windows password
#optional you'll be asked to enter your email.
5.	Edit the file docker-compose.yml by changing the port that you are going to use, according to your needs (port 80, for example, on my station was already in use by another service). After this update, start the lifecycle of the app using the docker-compose.yml and Dockerfile, by running:
docker-compose up
6.	The initial installation will take some time, and the last outputted line should be: "Attaching to container1name, container2Name" /// and all your containers will be listed here. We assume that container1name is the one that has the node server, and container2name is the one with the source code of your project.

You can now inspect your virtual environment by listing your containers:
docker container ls --all
CONTAINER ID        IMAGE                                             COMMAND                  CREATED
             STATUS              PORTS                                            NAMES
f92c9785b0d1        ...hub...com   "bash -c 'cd /var/ww…"   About an hour ago   Up 6 minutes        0.0.0.0:35545->35359/tcp, 0.0.0.0:80->9000/tcp   container1name
96f34dafa893        ubuntu:14.04                                      "bash -c 'mkdir -p /…"   About an hour ago   Up About an hour                                                     container2name

In this case the port 80 is used by the Windows system, for the container2name container and port 9000 is used by Docker’s container2name container. These ports are the ones that you previously configured in the docker-composer.yml file.

7.	Open another cmder (don't close the first one!) and connect as admin to the container2name container created previously:
docker exec -it container2name bash
This command executes bash inside the container (similar with ssh into a machine).

8.	Once inside the container, you can perform typical tasks like:
```
#these should be run on cold start (after a fresh container build)
npm install

and
grunt serve
```
9.	Open another cmder and run:
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" container2name

This will return the IP address of your Docker container machine. With previous Docker versions this IP address was used in the hosts file of the system to access our application from the host (your computer). However, with Docker installation on Windows 10 I do not use this IP address.

10.	Run cmder as Admin and change the C:/Windows/System32/drivers/etc/hosts, appending the mapping of your local site to the localhost IP address and also to Docker’s IP address:
```
cd C:\Windows\System32\drivers\etc
vim hosts
# Press insert
# at the end of hosts file, paste :
127.0.0.1  webdev

# press esc twice and ‘:wq’ to write and quit the edit 
```
11.	Open your favorite browser and access " http://webdev/ ". You should see the app running smoothly. 

Occasionally it happens that you get disconnected from the server with the docker image the you use to compose your containers. The error message looks something similar: "ERROR: Error: image ... not found". In this case you have to login again. To login run:
 docker login ...hub...com, where ...hub...com should be the location of your Docker image.


# Use Jade or Pug

npm i jade // to use later to transpile jade into html or js
jade -P jadetemplate.jade //transpile jade into html
jade --client --no-debug jadetemplate.jade //transpile jade into js

- use the | to mix more tags

// -- I'm a comm that dont shows in html
//  I'm a comm and I'll show in html

- varMe = 'I am a var'
p I am a paragraph containing the #{varMe}

...

# code-academy-notes
Code Academy 2017 lectures notes
