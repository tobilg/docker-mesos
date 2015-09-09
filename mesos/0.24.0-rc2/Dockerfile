FROM tobilg/iwyu-build
MAINTAINER tobilg <fb.tools.github@gmail.com>

# Install packages
RUN apt-get update && apt-get install  -yq --no-install-recommends --force-yes \
    openjdk-7-jdk \
	libcurl4-nss-dev \
	libsasl2-dev \
	libsasl2-2 \
	libapr1-dev \
	libsvn-dev \
	libsasl2-modules \
	autoconf \
    automake \
    sasl2-bin \
    libtool \
	maven \
	wget && \
    rm -rf /var/lib/apt/lists/*
	
# Set Mesos version
ENV MESOS_VERSION 0.24.0-rc2

# Export environment
ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-amd64
ENV LD_LIBRARY_PATH /usr/local/lib
	
# Donwload Mesos release
RUN wget https://github.com/apache/mesos/archive/$MESOS_VERSION.tar.gz && \
    tar -zxf $MESOS_VERSION.tar.gz

# Bootstrap & Configure
RUN cd mesos-$MESOS_VERSION && ./bootstrap && \
    mkdir build && \
    cd build && \
    ../configure --disable-python --disable-optimize --without-included-zookeeper

#Make, Install & Clean up in one step
RUN cd /mesos-$MESOS_VERSION/build && \
    make -j4 && \
    make install && \
    rm -rf /mesos-$MESOS_VERSION
