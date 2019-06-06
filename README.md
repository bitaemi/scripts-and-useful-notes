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
```
docker login ...hub....com
#Username: Enter your windows username
#Password: your windows password
#optional you'll be asked to enter your email.
```

5.	Edit the file docker-compose.yml by changing the port that you are going to use, according to your needs (port 80, for example, on my station was already in use by another service). After this update, start the lifecycle of the app using the docker-compose.yml and Dockerfile, by running:
```
docker-compose up
```

6.	The initial installation will take some time, and the last outputted line should be: "Attaching to container1name, container2Name" /// and all your containers will be listed here. We assume that container1name is the one that has the node server, and container2name is the one with the source code of your project.

You can now inspect your virtual environment by listing your containers:

```
docker container ls --all
CONTAINER ID        IMAGE                                             COMMAND                  CREATED
             STATUS              PORTS                                            NAMES
f92c9785b0d1        ...hub...com   "bash -c 'cd /var/ww…"   About an hour ago   Up 6 minutes        0.0.0.0:35545->35359/tcp, 0.0.0.0:80->9000/tcp   container1name
96f34dafa893        ubuntu:14.04                                      "bash -c 'mkdir -p /…"   About an hour ago   Up About an hour                                                     container2name

```

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

12.Now let's work with docker's containers and other cmds:

WIP = label in git work in progress


docker container logs docker_aweb_ng_build_watch_1 -f

docker container resta docker_aweb_ng_build_watch_1
make build-site
make mysql-restore DATABASE=ets_ezpublish
ls -lah /var/dumps/
cd ~workspace/ets/docker/

mv Downloads/ets_ezpublish.sql.gz /var/dumps/


docker-compose start -d // aceasta comanda a durat fff mult

daca am dat shot down la calc si implicit am oprit si docker:
{
docker-compose up -d// start in detached mode - tre sa-l rulez in folderul de docker-ca sa pornesc toate containerele, ci nu in folderul unui proj pt ca voi porni doar containerele din acel prj
docker container restart docker_aweb_ng_build_watch_1
}


-----------------------------------------------
dupa ce am dat deja push la modificarile mele pae branch  din phpStorm, trec in consola si :

git pull --rebase origin master
git status // dupa ma duc in editor si editez sectiunea de conflict din fisierele care au conflicte
git rebase --continue
git push --force-with-lease
git status

------------------------------------------------------------------------------------

docker fix to send emails:

git checkout fix-supervisor-config
docker-compose up -d --build --no-deps tes_consumers

---------------------------------

uneori trebuie sa dau restart la consumeri 

git checkout local-dump-cache // sa trec pe un branch astfel incat sa folosesc importul local de baze de date - 
diferenta era ca in fisierul Makefile al dockerului este actualizata reteta



docker container restart docker_aweb_ng_build_watch_1
docker container restart docker_emo_1
docker container restart docker_emo_gulp_watch_1
docker container logs docker_emo_gulp_watch_1
docker container logs docker_tes_1
docker container logs docker_tes_1  -f // nu a returnat nimic, de aceea dau exec 
docker container exec -it docker_tes_1 ash
tail -f var/logs/  //acum pot sa vad ce am pe cozile de rabbitMQ si ies din modul www cu comanda exit

idem pt:

docker container exec -it docker_nginx_1 ash

or 
tail -f var/logs/*


si trebuie sa am rabdare sa porneasca containere pentru ca dureaza


---------------------------------------------------------

docker container logs docker_aweb_ng_build_watch_1 -f //este error logul - pt development

docker container logs docker_aweb_ng_build_watch_1 -f
 docker container restart docker_aweb_ng_build_watch_1
docker container exec -it docker_nginx_1 bash sau docker container exec -it docker_nginx_1 ash
docker container logs docker_nginx_1 -f
cd projects/AWEB


docker container stop docker_emo_consumers_1 //asta dupa ce au updatat cei de la GroupEleven Symfony


sudo ln -sf /home/andreia/opt/PhpStorm-fdsdgfdfgfg/bin/phpstorm.sh /usr/local/bin/phpstorm //create a symlink for Phpstorm
after this when I am in home directory I can start phpstorm whit the command phpstorm

ps aux | grep Php
sudo apt install htop
htop //see which processes are running on linux
phpstorm & //to run the app in the background
________________________________________________________

500 error server not found + many errors of Symfony (emo container) - this is because I my PC crushed and I had to stop the docker

 docker container logs docker_nginx_1 -f
ps aux
htop //see which processes are running
docker-compose stop
sudo shotdown -r now //asta mi-a inchis SO pentru ca s-a blocat


the docker doesn't start because of some errors or code update made - the project cannot be build
so, the solution to fix docker si not to build with the existing config from Makefile, but as follow:

docker container ls --all //list all containers, not only the ones running
docker container logs docker_redis_1 -f // problema este probabil la cache=redis, cumva din pricina asta nu face build pentru ca sunt conflicte intre cache si resurse - cum aveam la Symfony, cand trebuia sa dau cache clear si composer.phar install sau update




cd workspace/ets/docker/
ll
cat .env
mv .env .env_bkp
git status // to see the differences of Makefile
git diff Makefile
git checkout .
git status
git checkout master
git pull --rebase
clear
cp .env.dist .env
make git-clone-all
cd projects/PREPTOOL/
docker container ls
make start-all-consumers
make stop-all-consumers
make create-all-queues
make composer-installs
make composer-install SERVICE=preptool
make composer-installs
make create-all-queues
docker-compose up
docker-compose up -d

git pull --rebase
git checkout stable
make git-clone-all
cd projects/PREPTOOL/


docker container exec -it docker_emo_1 ash // sa fac ssh pe container
docker container exec -it docker_preptool_1 ash
docker container stop docker_emo_consumers_1 //opresc containerul consumerilor pt ca ocupa mult
docker container exec -it docker_emo_1 ash
 

docker container logs docker_nginx_1 -f

---------------------------------------------------------

cat /etc/*-release //see your linux version

install a beter terminal:
sudo add-apt-repository ppa:webupd8team/terminix
sudo apt update
sudo apt install tilix

_______________________________________________________________________________

cand s-a schimbat versiunea de npm rxjs incompatibility pb :

docker-compose up -d --build aweb_ng_build_watch//docker_aweb_ng_build_watch_1 is up-to-date asadar va conecte am incercat sa rebuiduiesc containerul, insa nu a fost de ajuns, asadar a trebuit sa dau ssh pe containerul de AWEB si sa instalez npm

dAWEB //am folosit aliasul de ssh la container
npm install //am instalat node package manager ca sa aiba grija de dependente


-------------------------------------------------------


PhpStorm shortcuts
________________________________________________________________
just a youtube tutorial:

docker ps // list all containers runing
docker ps -a //list stoped containers
docker images // see the images that are present localy and use imediatly to start a container
// use the same image to start an environment with the same config
I can have more containers with the same image 
container(object) is a runing instance of an image(class) = the definition of an environment

Installing docker:

getdocker.com

containers very eficient because they share sharing resurses ( kernel) but they look like isolated environments

docker --version
docker-compose --version

docker pull nginx //will download the image


docker run --name my-nginx -p 80:80 nginx:1.10.2-alpine

// run the image for nginx alpine and start the container of it, named my-nginx while mapping port 80 to a 80 for this container I instantiate

- if I need to start the container with different parameters, then I have to remove the container I'm not using and use the same container name to create another one

docker rm my-nginx
_________________________________________________________________________________________________--


git stash//imi salvez modificarile facute pe acest branch  in stiva, inainte de a comuta pe alt branch
git checkout -//trec pe vechiul branch
trec pe branchul pe care vreau sa-l mergeuiesc = sa i fac rebase cu master

git pull // aduc de pe serverul de git  toate branchurile
git pull --rebase origin master // in loc de merge voi face rebase in acest caz
git status
git add .//adaug tot pt commit, dupa ce mi-am reparat conflictele
git rebase --continue
git push --force-with-lease
git checkout EB-8307
git pull
git stash pop 1
git reset --hard HEAD
git stash apply stash@{1}
git stash apply stash@{0} // dupa ce am revenit pe branch imi aduc modificarile salvate in stash
git stash clear // cu asta imi pierd salvarile de modificari pe care eu mi le-am pus in stash cand am trecut pe un alt branch
git stash list

-------------------------------------------------------------------------------------------------
Ansible este un packet de linux pt deployuri - am configuratia si il folosesc la deployul cu jenkins 

creez un job, il configurez cu branchul pe care vreau sa-l folosesc pt deploy si il folosesc pentru a da deploy

------------------------------------------------------

cd projects/EMO
docker container exec -it docker_emo_1 ash

cat /proc/sys/fs/inotify/max_user_watches
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf

-----------------------------------------

daca imi crapa requesturi de la apiuri de emo sau altele tre sa rulez migrarile:

ssh pe masina=containerul aferent proiectului

docker container exec -it docker_emo_1 ash
app/console doctrine:migra:migr  //nu e nevoie sa scriu complet pt ca executabilul parseaza si comenzile partiale
composer install --no-scripts //sa actualizez composerul dipa ce am facut migrarea

-------------------------------------------

typora - editor text!!! folosesc markdown #" ... si scripu textul

Ctrl+K pe slack iti deschide search in slack


------------------------------------------------

rm -rf var/cache

???sudo chown andreia:andreia - R


http://127.0.0.1:15672/#/exchanges

ca sa scalam consumerii de accea avem microserviciul session consumers care are config diferit de microserviciul session, dar foloseste acelasi codebase

ctrl+alt+backspace //restart sesiunea de linux interface cand mi s-a blocat


----------------------------------------------------------------
git config --global core.editor "vim"

git rebase -i HEAD~6
acum se deschide cu vim sectiunea unde in loc de pick scriu s ( prescurtarea de la squash) pentru toate commiturile exceptandul pe cel pe care vreau sa il pastrez si in care vreau sa inglobez toate celelalte commituri in dreptul carora scriu squash

dupa scriu :wq  //write and quit the vim editor

git push --force-with-lease




ctrl+R si incep sa scriu din commanda pe care o vreau



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
# Debuging

If you want to see the json data transmited via HTTPrequests, apend the string: ?onlyData=true to the bowser URL link and after
that write in the consola JSON.stringify(pageData) to see the Json object.

# code-academy-notes
Code Academy 2017 lectures notes
