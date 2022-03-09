---


# What is it ?

A docker image for easily using RQDA. RQDA is an awesome Gtk+ qualitative data analysis tools for R (a "computer-aided qualitative data analysis", CAQDA).

Docker, container, image ?? *A container image is a lightweight, stand-alone, executable package of a piece of software that includes everything needed to run it: code, runtime, system tools, system libraries, settings.* ([Official doc](https://www.docker.com/what-container))

More on docker on [Pokyah's blog](https://pokyah.github.io/howto/using-r-with-docker/).


# Why ?

This project find its roots in the fact that RQDA dependencies are not easily available/installed in modern distribution of Linux and R.


# How ?

This image proposes the last stable version of RQDA (0.2-8) running in Debian 8 (jessie) and R (3.1)

## Ubuntu (X11)

```bash
set -o allexport
source .env
set +o allexport
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -

#Build the image 
docker-compose build

#Run 
docker-compose up -d

#First use : 
docker exec -it "${PWD##*/}_rqda_1" R CMD INSTALL --no-test-load /root/gWidgetsRGtk2_0.0-86.1.tar.gz
docker exec -it "${PWD##*/}_rqda_1" R CMD INSTALL --no-test-load /root/RQDA_0.2-8.tar.gz

#Then  
docker exec -it "${PWD##*/}_rqda_1" R
#Start the app in R : library(RQDA)
```

## MacOS (XQuartz)

Télécharger et installer XQuartz (https://www.xquartz.org/)

```bash
set -o allexport
source .env
set +o allexport
touch $XAUTH
touch $XSOCK
sudo ifconfig lo0 alias 1.1.1.1
xhost + 1.1.1.1

#Build the image 
docker-compose -f docker-compose-mac.yml build

#Run 
docker-compose -f docker-compose-mac.yml up -d

#First use : 
docker exec -it "${PWD##*/}-rqda-1" R CMD INSTALL --no-test-load /root/gWidgetsRGtk2_0.0-86.1.tar.gz
docker exec -it "${PWD##*/}-rqda-1" R CMD INSTALL --no-test-load /root/RQDA_0.2-8.tar.gz

#Then  
docker exec -it "${PWD##*/}-rqda-1" R
#Start the app in R : library(RQDA)
```


# Credits & More info

RQDA is available under [bsd-license](http://rqda.r-forge.r-project.org/License.html). It was coded by [Ronggui HUANG](https://github.com/Ronggui).

Documentations, tutorials, how-tos and other ressources on RQDA can be found on the [main website of the project](http://rqda.r-forge.r-project.org/). 
Devel version of RQDA is in progress on GitHub : <https://github.com/Ronggui/RQDA>

HUANG Ronggui (2016). RQDA: R-based Qualitative Data Analysis. R package version 0.2-8.


# References

Among [references](http://rqda.r-forge.r-project.org/publications.html), I used RQDA for two scientific papers : 

-   [Development of a broadened cognitive mapping approach for analysing systems of practices in social–ecological systems ](https://doi.org/10.1016/j.ecolmodel.2012.11.023)
-   [A new approach for comparing and categorizing farmers’ systems of practice based on cognitive mapping and graph theory indicators](https://doi.org/10.1016/j.ecolmodel.2013.11.026)

