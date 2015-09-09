# Mesos
This is a build of the lastest Mesos version available in the [Mesos Git repository](https://github.com/apache/mesos). Currently, the latest version is 0.24.0-rc2. 
The intention of creating this image was to be able to provide and use the latest Mesos versions not yet available via the package managers.

It is built using the `tobilg/iwyu-build` Docker image, without Python support, but with Java support. The local Zookeeper is also deactivated.

Build flag used are `--disable-python --disable-optimize --without-included-zookeeper`.

### Master & Slave Dockerfiles
Have a look at the `docker-master` and `docker-slave` subfolders of this repository to find runnable Dockerfiles. 
