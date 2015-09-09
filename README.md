# Dockerfiles for Apache Mesos
This repository contains builds of the lastest Mesos versions available in the [Mesos Git repository](https://github.com/apache/mesos). Currently, the latest version is 0.24.0-rc2. 
The intention of creating this image was to be able to provide and use the latest Mesos versions not yet available via the package managers.

### Master & Slave Dockerfiles
Have a look at the `docker-master` and `docker-slave` subfolders of this repository to find runnable Dockerfiles. 

### Automated builds 

These images are automatically built and synced to the Docker Hub Registry at
[tobilg/mesos-master](https://registry.hub.docker.com/u/tobilg/mesos-master/) and
[tobilg/mesos-slave](https://registry.hub.docker.com/u/tobilg/mesos-slave/).
