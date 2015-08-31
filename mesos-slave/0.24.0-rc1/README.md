#Mesos Slave

This Docker image is using the base `tobilg/mesos` image, which is a build of the lastest Mesos version available in the [Mesos Git repository](https://github.com/apache/mesos). 
Currently, the latest version is 0.24.0-rc1.

## Configuration

Mesos allows you to set all [configuration
options](http://mesos.apache.org/documentation/latest/configuration/) via
environment variables.  That means that you don't need any additional scripts to
start up a Mesos slave node using this Docker image; just pass in `-e` options
to set each configuration option:

    $ docker run -d \
        --name mesos_slave \
        -e MESOS_LOG_DIR=/var/log/mesos/slave \
        -e MESOS_WORK_DIR=/var/lib/mesos/slave \
        -e MESOS_MASTER=[zookeeper URL] \
        -p 5051:5051 \
        tobilg/mesos-slave

Also note the `-p` option, which exposes the default Mesos slave TCP port
(5051) on the container host.

### Running containerizers

You can use the containerizers (Mesos, Docker) by providing the following options:

    $ docker run -d \
        --name mesos_slave \
        --privileged \
        -e MESOS_LOG_DIR=/var/log/mesos/slave \
        -e MESOS_MASTER=[zookeeper URL] \
        -e MESOS_EXECUTOR_REGISTRATION_TIMEOUT=5mins \
        -e MESOS_ISOLATON=cgroups/cpu,cgroups/mem \
        -e MESOS_CONTAINERIZERS=docker,mesos \
        -v /run/docker.sock:/run/docker.sock \
        -v /sys:/sys \
        -v /proc:/proc \
        -p 5051:5051 \
        tobilg/mesos-slave

### Networking

If you want your Mesos slave to be accessible via the Docker host ip, then you should add the following options:

        --net=host \
        -e MESOS_IP=$(/usr/bin/ip -o -4 addr list eth0 | grep global | awk '{print $4}' | cut -d -f1) \
        -e MESOS_HOSTNAME=$(/usr/bin/hostname) \

Be sure to replace `eth0` with the actual interface your host is using for external access. For RedHat/CentOS/Fedora-based hosts' the `MESOS_IP` line needs to replaced with

        -e MESOS_IP=$(/sbin/ifconfig eth0 | grep 'inet ' | awk '{print $2}') \