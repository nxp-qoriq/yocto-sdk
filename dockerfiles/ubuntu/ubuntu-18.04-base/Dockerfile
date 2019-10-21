FROM ubuntu:18.04

ARG http_proxy
ENV http_proxy $http_proxy
ARG https_proxy
ENV https_proxy $https_proxy

RUN apt-get update && apt-get -y install gawk wget git-core diffstat unzip texinfo gcc-multilib \
    build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
    xz-utils debianutils iputils-ping libsdl1.2-dev xterm tar locales curl screen

RUN rm /bin/sh && ln -s bash /bin/sh

RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo && chmod a+x /usr/bin/repo

RUN locale-gen en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV USER_NAME edgescale

ARG host_uid=1001
ARG host_gid=1001
RUN groupadd -g $host_gid $USER_NAME && useradd -g $host_gid -m -s /bin/bash -u $host_uid $USER_NAME

USER $USER_NAME 

ENV YOCTO_WORKDIR /home/$USER_NAME/yocto-sdk
RUN mkdir -p $YOCTO_WORKDIR

WORKDIR $YOCTO_WORKDIR
RUN repo init -u https://source.codeaurora.org/external/qoriq/qoriq-components/yocto-sdk -b refs/tags/yocto_2.6_es_1906 && repo sync --no-clone-bundle

CMD /bin/bash
