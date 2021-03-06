
FROM centos:centos6
MAINTAINER Traun Leyden <tleyden@couchbase.com>

ENV CB_VERSION		2.2.0
ENV CB_RELEASE_URL	http://packages.couchbase.com/releases
ENV CB_PACKAGE		couchbase-server-community_2.2.0_x86_64.rpm
ENV GOPATH /opt/go
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$GOPATH/bin

# Create directory to hold go code
RUN mkdir -p $GOPATH

# Install yum dependencies
RUN yum install -y \
    wget 

# Install EPEL, so that we can install golang
RUN cd /tmp && \ 
    wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm && \
    rpm -ivh epel-release-6-8.noarch.rpm

# Install more yum dependencies
RUN yum install -y \
    git \
    golang \
    hg \
    hostname \
    initscripts \
    openssl \
    pkgconfig \
    tar \
    wget 

# Install couchbase
RUN rpm --install $CB_RELEASE_URL/$CB_VERSION/$CB_PACKAGE

# Copy couchbase binaries into a dir in the path
# (you would think installing couchbase would do this for you)
# I haven't figured out how to append to $PATH, hence I'm doing it this way
RUN cp -r /opt/couchbase/bin/* /usr/local/bin && \
    cp -r /opt/couchbase/bin/tools/* /usr/local/bin && \
    cp -r /opt/couchbase/bin/install/* /usr/local/bin

# Install Godep + couchbase-cluster-go
RUN go get -u -v github.com/tools/godep && \
    godep get github.com/tleyden/couchbase-cluster-go/...

# Put start script
ADD scripts/couchbase-cluster-wrapper /usr/local/bin/

