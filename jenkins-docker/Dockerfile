FROM jenkins/jenkins:lts

#install Jenkins plugins
RUN /usr/local/bin/install-plugins.sh \
    build-with-parameters \ #RQ02, RQ08: Parameters required
    discard-old-build \     #RQ01: Keep only 10 builds
    docker-plugin \         #RQ05: Run inside docker
    github-branch-source \  #The code is stored on github
    workflow-aggregator \   #Pipeline is needed
    ws-cleanup              #RQ10: Clean up

# We need docker inside of the container to run builds inside (not best practice! Only for proof of concept!) 
USER root

RUN echo "deb http://ftp.de.debian.org/debian/ stretch main" > /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y \
        dmsetup \
        iptables \
        libdevmapper1.02.1 \
        libip4tc0 \
        libip6tc0 \
        libiptc0 \
        libltdl7 \
        libnetfilter-conntrack3 \
        libnfnetlink0 \
        libseccomp2 \
        libxtables12 \
        sudo \
    && rm -rf /var/lib/apt/lists/*
 
RUN wget https://download.docker.com/linux/debian/dists/stretch/pool/stable/amd64/docker-ce_18.03.1~ce-0~debian_amd64.deb \
    && dpkg -i docker-ce_18.03.1~ce-0~debian_amd64.deb \
    && rm docker-ce_18.03.1~ce-0~debian_amd64.deb \
    && echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers \
    && usermod -a -G docker jenkins

USER jenkins
