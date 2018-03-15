# 2018 computer networks lab


## Requirements
- [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [docker-compose](https://docs.docker.com/compose/install/) already in this repo
- [University of Washington - Computer Networks](https://www.youtube.com/watch?v=xKNPTYtTnAo&list=PLfgkuLYEOvGMWvHRgFAcjN_p3Nzbs1t1C)


## Concepte de baza
O masina virtuala de docker o vom numi container sau serviciu. Pentru a porni o masina virtuala, trebuie sa definim o imagine cu un sistem de operare care sa fie utilizat pe acea masina. Imaginea o vom construi folosind [docker build](https://docs.docker.com/engine/reference/commandline/build/), imagine care va avea tag-ul *baseimage*. Aceasta este construita pe baza fisierului [./docker/Dockerfile](https://github.com/senisioi/computer-networks/blob/master/docker/Dockerfile) care defineste ce sistem de operare folosim, ce aplicatii vor fi instalate si ce useri vor exista pe containerele care ruleaza acea imagine. Dupa ce s-a construit imaginea, folosim [docker-compose up -d](https://docs.docker.com/compose/reference/up/) pentru a lansa mai multe containere care sunt definite intr-un fisier numit [docker-compose.yml](https://github.com/senisioi/computer-networks/blob/master/docker-compose.yml). Fisierul din umra trebuie sa se gaseasca in acelasi path cu cel in care rulam docker-compose. Daca deschidem acest fisier, vedem mai multe containere definite in sectiunea *services*: rt1, rt2, etc., containere care sunt configurate sa ruleze o imagine data (in cazul nostru *baseimage*, imaginea construita la pasul anterior), sa fie conectate la o retea (in cazul nostru reteaua *dmz*) sau sa aiba definite [un mount](https://unix.stackexchange.com/questions/3192/what-is-meant-by-mounting-a-device-in-linux) local.
Comanda docker-compose pe linux nu se instaleaza default cu docker, ci trebuie [sa o instalam separat](https://docs.docker.com/compose/install/). In cazul nostru, comanda se gaseste chiar in directorul computer-networks, in acest repository. Asa ca daca nu aveti comanda instalata, o puteti rula din acest director folosind calea relativa `./docker-compose`.


## Starting up
```bash
# build the docker image
docker build -t baseimage ./docker/
# start services defined in docker-compose.yml
docker-compose up -d
```


## Basic docker commands
```bash
# stop services
docker-compose down

# see the containers running
docker ps

# kill a container
docker kill $CONTAINER_ID

# see the containers not running
docker ps --filter "status=exited"

# remove the container
docker rm $CONTAINER_ID

# list available networks
docker network ls

# inspect network
docker network inspect $NETWORK_ID

# inspect container
docker inspect $CONTAINER_ID

# see container ip
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $CONTAINER_ID

# attach to a container
docker exec -it $CONTAINER_ID bash

# attach using docker-compose
docker-compose exec rt1 bash

# attach as root to a container
docker-compose exec --user root rt1 bash
```

## References
- [docker concepts](https://docs.docker.com/engine/docker-overview/#docker-engine)
- [docker-compose](http://docker-k8s-lab.readthedocs.io/en/latest/docker/docker-compose.html)
- [compose networking](https://runnable.com/docker/docker-compose-networking)
- [extra](https://success.docker.com/article/Docker_Reference_Architecture-_Designing_Scalable,_Portable_Docker_Container_Networks)
