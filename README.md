# HygieiaLight
A light weight Hygieia setup
Based on https://github.com/capitalone/Hygieia

The current Hygieia documentation is not very clear and the setup process mentioned there are somewhat broken.
This repository lists out how I was able to tackle some of those challenges.

The current challenges with the Hygieia documentation and code are:
- The [release tags](https://github.com/capitalone/Hygieia/releases) don't have release artifacts and hence prone to getting broken by anyone pushing broken changes to it. Released maven artifact versions must not contain "-SNAPSHOT".
- There is no maven repository for the artifacts which are released. Hence it needs everyone to deal with code build instead of focusing mainly at how to quickly setup and use it.
- Image build process is complicated. Created ticket [here](https://github.com/capitalone/Hygieia/issues/1701) but never heard from the maintainers.
- Given this is a fantastic tool/framework, the code is not structured enough to follow the design principles of a good framework.


# Docker Images
As of now I have created images and uploaded to my dockerhub account.

You can find them at [my Hygieia images](https://cloud.docker.com/swarm/pravsingh/repository/list?name=hygieia&namespace=pravsingh&page=1)

# Running Hygieia
The docker-compose.yml file used my published images and hence you don't have to close and build original Hygieia images from the source code. Below will start the Hygieia API, UI, MongoDB and other connectors needed.

docker-compose up

should launch UI at http://localhost:8088

# How to build images using the code
clone Hygieia using:
```bash
git clone git@github.com:capitalone/Hygieia.git

cd Hygieia
```
 - the below command will build all the available images one by one and might take more than 30 mins to complete:
```bash
find . | grep Dockerfile | sed -e 's#/docker/Dockerfile##g' | awk '{print "mvn clean package -pl "$1" docker:build";}' | grep -v "/target" | sh
```
# Tagging and pushing the images under your account
I'll use my user id 'pravsingh' in dockerhub:

- tag local images with your user id
```bash
docker images | grep -e '^hygieia.*' | awk '{print "docker tag "$1":"$2" pravsingh/"$1;}' | sh
```
- make sure you login to docker first
```bash
docker login
```
- push the images to your dockerhub account
```bash
docker images | grep -e '^hygieia.*' | awk '{print "docker push pravsingh/"$1;}' | sh
```
# Creating DB & user in mongodb

The image "pravsingh/hygieia-mongodb" already has DB & user already created with DB Name=db, User Name=db, Password=dbpass.
If you want to create something different, here are the steps:

```bash
docker run -d -p 27017:27017 --name mongodb -v mongo:/data/db pravsingh/hygieia-mongodb  mongod --smallfiles
docker exec -it mongodb mongo admin
```
-----type below in the command console
```bash
use db
db.createUser({user: "db", pwd: "dbpass", roles: [{role: "readWrite", db: "dashboard"}]})
exit
```

