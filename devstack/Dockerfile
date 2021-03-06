FROM ubuntu:16.10

# noninteractive
ENV DEBIAN_FRONTEND noninteractive

# policy-rc.d likes to block devstack 
RUN perl -pi -w -e 's/101/0/g;' /usr/sbin/policy-rc.d 

# install required files
RUN apt-get update && \
	apt-get -y install \
	python \
	net-tools \
	bsdmainutils \
	git \
	build-essential \
	libssl-dev \
	libffi-dev \
	python-dev \
	sudo \
	vim \
	apt-utils \
	lsb-release \
	openvswitch-switch \
	supervisor \
    mysql-client \
	openssh-server

RUN mkdir /github

WORKDIR /github

RUN git clone https://git.openstack.org/openstack-dev/devstack 

WORKDIR /github/devstack

RUN tools/create-stack-user.sh && \
	chown -R stack:stack /github

#USER stack

COPY local.conf /github/devstack/
COPY /patches/* /github/devstack/
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# apply patch
RUN git apply *.patch

EXPOSE 22 80 443

CMD ["/usr/bin/supervisord"]
