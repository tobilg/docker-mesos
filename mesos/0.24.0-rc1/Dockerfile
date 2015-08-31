FROM tobilg/iwyu-build
MAINTAINER tobilg <fb.tools.github@gmail.com>

RUN apt-get update && apt-get install  -yq --no-install-recommends --force-yes \
    openjdk-6-jdk \
    libcurl4-nss-dev \
    libsasl2-dev \
    libapr1-dev \
    libsvn-dev \
    autoconf \
    automake \
    libsasl2-2 \
    sasl2-bin \
    libsasl2-modules \
    libtool \
    wget \
    git \
    maven 

#Clone mesos repository
RUN git clone --branch 0.24.0-rc1 http://git-wip-us.apache.org/repos/asf/mesos.git

#Bootstrap
RUN cd mesos && ./bootstrap

#Configure
RUN mkdir /mesos/build && cd mesos/build && ../configure --disable-python --disable-optimize --without-included-zookeeper

#Make
RUN cd /mesos/build && make

#Install
RUN cd /mesos/build && make install

#Include libmesos.so
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

#Clean up
RUN rm -rf /mesos
