FROM centos:7
MAINTAINER Maxfield Stewart

# Install Packages
RUN yum install -y git && \
    yum install -y wget && \
	yum install -y openssh-server && \
	yum install -y java-1.8.0-openjdk && \
	yum install -y sudo && \
	yum install -y which && \
	yum clean all

# Create Jenkins User
RUN useradd jenkins -m -s /bin/bash

RUN chown -R jenkins /home/jenkins
RUN chgrp -R jenkins /home/jenkins

# Add the jenkins user to sudoers
RUN echo "jenkins    ALL=(ALL)    ALL" >> etc/sudoers

# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf

RUN yum install -y yum-utils \
    && yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo \
    && yum makecache fast \
    && yum install -y docker-ce sysvinit-tools

# Enable jenkins user use doker
RUN gpasswd -a jenkins docker

ENV DIND_COMMIT 3b5fac462d21ca164b3778647420016315289034

RUN wget "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind" -O /usr/local/bin/dind \
	&& chmod +x /usr/local/bin/dind

VOLUME /var/lib/docker

COPY dockerd-entrypoint.sh /etc/dockerd-entrypoint.sh
RUN chmod +x /etc/dockerd-entrypoint.sh

CMD [ "/etc/idockerd-entrypoint.sh" ]

