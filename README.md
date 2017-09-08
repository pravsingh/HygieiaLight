# HygieiaLight
A light weight Hygieia setup
Based on https://github.com/capitalone/Hygieia

The current Hygieia documentation is not very clear and the setup process mentioned there are somewhat broken.
This repository lists out how I was able to tackle some of those challenges.

The current challenges with the Hygieia documentation and code are:
- The (release tags)[https://github.com/capitalone/Hygieia/releases] don't have release artifacts and hence prone to getting broken by anyone pushing broken changes to it. Released maven artifact versions must not contain "-SNAPSHOT".
- There is no maven repository for the artifacts which are released. Hence it needs everyone to deal with code build instead of focusing mainly at how to quickly setup and use it.
- Image build process is complicated. Created ticket (here)[https://github.com/capitalone/Hygieia/issues/1701] but never heard from the maintainers.
- Given this is a fantastic tool/framework, the code is not structured enough to follow the design principles of a good framework.


# Docker Images
As of now I have created images and uploaded to my dockerhub account.
You can find them at https://cloud.docker.com/swarm/pravsingh/repository/list?name=hygieia&namespace=pravsingh&page=1

