#Mesos Master

This Docker image is using the base `tobilg/mesos` image, which is a build of the lastest Mesos version available in the [Mesos Git repository](https://github.com/apache/mesos). 
Currently, the latest version is 0.22.1.

## Configuration

Mesos allows you to set all [configuration
options](http://mesos.apache.org/documentation/latest/configuration/) via
environment variables.  That means that you don't need any additional scripts to
start up a Mesos master node using this Docker image; just pass in `-e` options
to set each configuration option:

    $ docker run -d \
        --name mesos_master \
        -e MESOS_LOG_DIR=/var/log/mesos/master \
        -e MESOS_WORK_DIR=/var/lib/mesos/master \
        -e MESOS_ZK=[zookeeper URL] \
        -p 5050:5050 \
        tobilg/mesos-master

Also note the `-p` option, which exposes the default Mesos master TCP port
(5050) on the container host.

### Clustering

If you want to run multiple Mesos masters, then you have to additionally specify the quorum size. The size can be calculated as `quorum > (number of masters)/2`, e.g.

    -e MESOS_QUORUM=2

if you have 3 master nodes. Also, you should add a cluster name:

    -e MESOS_CLUSTER=[Your cluster name]

### Networking

If you want your Mesos master to be accessible via the Docker host ip, then you should run it with the following options (on Debian/Ubuntu etc. based hosts):

    $ docker run -d \
        --name mesos_master \
        --net=host \
        -e MESOS_IP=$(/usr/bin/ip -o -4 addr list eth0 | grep global | awk '{print $4}' | cut -d -f1) \
        -e MESOS_LOG_DIR=/var/log \
        -e MESOS_WORK_DIR=/var/lib/mesos/master \
        -e MESOS_ZK=[zookeeper URL] \
        -p 5050:5050 \
        tobilg/mesos-master

Be sure to replace `eth0` with the actual interface your host is using for external access. For RedHat/CentOS/Fedora-based hosts' the `MESOS_IP` line needs to replaced with

    -e MESOS_IP=$(/sbin/ifconfig eth0 | grep 'inet ' | awk '{print $2}') \