# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM krallin/ubuntu-tini:trusty

MAINTAINER Diogo Melo <diogro@gmail.com>

RUN apt-get update && apt-get install -yq --no-install-recommends \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    git \
    zsh \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://mran.revolutionanalytics.com/install/mro/3.2.5/MRO-3.2.5-Ubuntu-14.4.x86_64.deb && \
    echo "f73378c5942621f8f49f7c28ce107608bfe71f808ba5bf58379437b2963b6894 MRO-3.2.5-Ubuntu-14.4.x86_64.deb" | sha256sum -c - && \
    dpkg -i MRO-3.2.5-Ubuntu-14.4.x86_64.deb && \
    rm MRO-3.2.5-Ubuntu-14.4.x86_64.deb

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

# Configure environment
ENV SHELL /bin/bash
ENV NB_USER diogro
ENV NB_UID 1000
ENV HOME /home/diogro
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Create diogro user with UID=1000 and in the 'users' group
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir -p /opt/conda && \
    chown jovyan /opt/conda

USER diogro

# Setup diogro home directory
RUN mkdir /home/$NB_USER/projects

USER root

# Configure container startup as root
EXPOSE 8888
WORKDIR /home/$NB_USER/
ENTRYPOINT ["tini", "--"]

# Switch back to diogro to avoid accidental container runs as root
USER diogro
