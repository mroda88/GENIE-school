# Using a docker

If none of the other solutions fit your case, you can use a docker image containing all the needed software.

## Requirements
* Git
* A terminal
* Internet connection


## Install Docker Desktop
The first step is the installation of docker. 

To do so, follow the instructions available on the docker wiki:
 * [Instruction for MacOS](https://docs.docker.com/desktop/install/mac-install/), be careful to select instructions for the CPU chip installed on your PC (Intel or Apple)
 * [Instruction for Windows](https://docs.docker.com/desktop/install/windows-install/), select WSL or Hyper-V instruction based on your preference. [Here](https://docs.docker.com/desktop/wsl/) there are more instructions on using docker with the WSL
 * [Instruction for Linux](https://docs.docker.com/desktop/install/linux-install/)

 After the installation, launch Docker Desktop. For the moment keep it open and forget about it.

## Get the code
With Docker Desktop open, start a terminal (standard one for Linux and Mac, [like this](https://docs.docker.com/desktop/wsl/use-wsl/) for Windows with WSL).
From the terminal, create a working directory, access it, and clone this repository:
```console
mkdir /your/working/folder
cd /your/working/folder
git clone https://github.com/mroda88/GENIE-school.git
```
This will create the `GENIE-school` folder, with all the needed scripts and files. Go to the `docker` folder inside it:
```console
cd GENIE-school/docker
```
In this folder, among other files, there is the dockerfile with all the instructions needed to install Genie and its dependencies.

## Build the image
From the docker folder, we can now start to build the image. First, since we haven't done it yet, check whether or not docker is running. Simply run the command
```console
docker
```
If docker is running you should get a long help message, starting with
<!-- to do: write what to do if docker is not running-->
```
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
 run         Create and run a new container from an image
 exec        Execute a command in a running container
 ps          List containers
 build       Build an image from a Dockerfile
 pull        Download an image from a registry
 push        Upload an image to a registry
 images      List images
 login       Log in to a registry
 logout      Log out from a registry
 search      Search Docker Hub for images
 version     Show the Docker version information
 info        Display system-wide information
```
If everything is going as expected, simply run
```console
docker build -t genie-inss .
```
This will create an image from the instructions in the dockerfile. It should look like:
```console
[+] Building 1229.1s (5/6) docker:desktop-linux
 => [internal] load .dockerignore
 => => transferring context: 65B 
 => [1/3] FROM docker.io/library/ubuntu:22.04@sha256:a6d2b38300ce017add71440577d5b0a90460d0e57fd7aec21dd0d1b0761bbfb2 0.0s
 => CACHED [2/3] RUN rm /bin/sh && ln -s /bin/bash /bin/sh &&    apt-get -y update &&    apt-get -y upgrade &&    apt-get -y install git &&    apt-get -y install dpkg-dev   0.0s
 => [3/3] RUN mkdir /INSS &&    cd /INSS &&    wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.5.4.tar.gz -O LHAPDF-6.5.4.tar.gz &&    tar xf LHAPDF-6.5.4.tar.gz  1227.3s
 ```
This will (slowly) go through all the instructions, installing all the software, setting all the environmental variables, and all this type of stuff.

## Exporting X11
With the image built, we could now run a container using it. Before doing so, we need to set the export of the display so that we can use graphical interfaces from within the container.

To do so, download an X11 server on your machine:

### Mac
 * Install the latest [XQuartz X11](https://www.xquartz.org/) server and run it
 * Activate the option ‘Allow connections from network clients’ in XQuartz settings
 * Quit & restart XQuartz (to activate the setting)

### Windows
 * Install MobaXterm
 * Run it

### Linux
 * To do (but it is probably already included)

With the server on, run
```console
xhost + 127.0.0.1
```

## Run the container
You can now run the container! Simply run
```console
docker run -e DISPLAY=host.docker.internal:0 -it --name genie-inss-container genie-inss
```

This will launch and enter the container. The shell should be something like this now:
```console
root@63bfd1d6f2e3:/INSS# 
```
In the INSS folder there will be few folders of all the installed software:
 * ROOT6
 * Pythia6
 * THIS
 * AND
 * THAT

From here, test if the display is working running
```console
root@730a97de8de2:/INSS# root -l
root [0] TBrowser f
```
if the TBrowser opens, great! We are done with the installation! Use the container as if it is a standard Ubuntu shell.

## Stop and restart the container
When you are done with Genie, close the container with either `ctrl+D` or by typing `exit`.

To restart the container and maintain all the work done, run
```console
docker start -i genie-inss-container
```

If you forgot to give a name to your container, run
```console
docker ps -a
```
this will print a list of all the containers run in the past. Look for the most recent and check its ID. With the ID, run
```console
docker start -i <containerID>
```


## Some debug info
Here is a (absolutely non-complete) list of the problems you could run into.

If you see that the build of your docker is failing, try to decrease the number of cores used for the ```make``` of your docker. So you can test ```make -j4``` or ```make -j1``` by editing the [Dockerfile](https://github.com/mroda88/GENIE-school/blob/119b86e5c6ef8908b1eff82cb091cf159f7ec310/docker/Dockerfile#L122). It could also be very important to reduce the use of your computer while doing this since ROOT compilation can be quite resource-demanding. An idea is to leave this run overnight and check on it in the morning. 

You can try to build a container from images that have already been build for you. 
You can find those directly from the search bar of your Docker desktop app.
- valerpia/genie-inss
- valerpia/genie-inss-amd64 (if you have a mac with a arm chip, try also this one if the other one did not work)

