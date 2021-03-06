# ICS5114 Big Data Processing &ndash; NoSQL Databases Workshop &ndash; Dr Jean-Paul Ebejer

During this workshop I will show you the basic concepts of NoSQL systems.  We 
will use four NoSQL technologies, each representing a different data model 
-- Memcached, MongoDB, Cassandra, and Neo4j.

## Requirements

The following assumes you have a base Linux (Ubuntu 16.04) installation.  If you 
are a windows fan, may I suggest you create a beefy VM (using VirtualBox or on 
the cloud) and install Ubuntu there.

You will need a ton of different software (configuration, etc.) for this workshop.
Normally, this would be a pain, but we will be using docker so everything is 
conveniently installed and set-up for you and placed in different docker 
containers (you're spoilt these days!).

The rest of the workshop (and practical) assumes you have docker installed.
This can be achieved readily (on Ubuntu 16.04.3 LTS) with:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo systemctl status docker
sudo usermod -aG docker ${USER}
```

## NoSQL Database Engines

We will use (the latest) official docker instances to install the NoSQL database
engines.  Follow the instructions presented in each section.  Each of these steps
may take a few minutes.  All the names of the containers will be prefixed with
`some-`.

### Memcached

To run memcached execute:

```
docker run --init --name some-memcache -d memcached
```

`-d` detaches the docker container once initialized.

### MongoDB

To run mongodb execute:

```
docker run --name some-mongo -d mongo
```

### Cassandra

To run cassandra execute:

```
docker run --name some-cassandra -d cassandra
```

### Neo4J

To run neo4j execute:

```
docker run --name some-neo4j --env=NEO4J_AUTH=none -p=7474:7474 -p=7687:7687 -d neo4j
```

Note that the above switches off Neo4J authetication (by setting an environment
variable in the container).

## Jupyter Lab 

I have developed example usages of each NoSQL database, using Jupyter lab.  This
is a browser-based IDE which uses Jupyter Notebooks.  First, you should copy **all** 
code (as Jupyter notebooks) [here](https://github.com/jp-uom/nosql_workshop/tree/master/jupyter) 
in a local directory on your Ubuntu installation.  Remember to change the 
`/your/local/path` path below.

```
docker run -ti --rm \
    --name nosql-workshop \
    --link some-memcache:memcache \
    --link some-mongo:mongo \
    --link some-cassandra:cassandra \
    --link some-neo4j:neo4j \
    -v /your/local/path:/notebooks \
    -p=8888:8888 \
    jpebe/nosql-workshop
```
This container is linked to all the containers we set up (and are running).

You should then be able to copy the Jupyter notebook URL from the terminal into 
your browser (ctrl-click will open a browser automatically).

    
## Limitations

The databases we run within the containers have no volume mounted (from Ubuntu).  
This means that all data created during our exercises is stored in the container 
which gets lost when you stop running the container.  If you want your data to 
persist, you have to mount volumes (on your Ubuntu installation) and use these 
as the data directory of the NoSQL engine.

Changes to the jupyter notebooks will, however, be persisted as these are mounted
from your own local directory.

## When you are done

You may want to reclaim some space on your Ubuntu installation.  Remove all images 
and containers in the following way.

```
docker rm -f $(docker ps -a -q)
docker rmi -f $(docker images -a -q)
```

<!--
```
docker pull jpebe/nosql
```
-->
<!--
```
docker run -ti --rm --name nosql-workshop -v /home/jp/cloud/google-drive-uom/lecturing/2017-2018/ICS5114_Big_Data_Processing/class_practicals/nosql/docker/jupyter:/notebooks --link some-memcache:memcache --link some-mongo:mongo --link some-cassandra:cassandra --link some-neo4j:neo4j -p=8888:8888 jpebe/nosql-workshop
```
-->


## Conclusion

**Pull requests (with fixes, even to this guide) will be recieved with appreciation and thanks.**

I hope you enjoy this session!

Dr Jean-Paul Ebejer
<br />

![](https://github.com/drmenguin/learnd/blob/master/jp.gif)
