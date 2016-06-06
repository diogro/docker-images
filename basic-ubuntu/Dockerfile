# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM krallin/ubuntu-tini:trusty

MAINTAINER Diogo Melo <diogro@gmail.com>

# Basic stuff
RUN apt-get update && apt-get install -yq --no-install-recommends \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    git \
    zsh \
    vim \
	curl \
	software-properties-common \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# R dependencies
RUN apt-get update && apt-get install -yq --no-install-recommends \
    libcairo2 libgfortran3 libglib2.0-0 libjpeg8 libpango-1.0-0 libpangocairo-1.0-0 \
    libtcl8.6 libtiff5 libtk8.6 libx11-6 libxt6 gfortran fonts-dejavu libblas-dev liblapack-dev\
	libx11-dev mesa-common-dev libglu1-mesa-dev libcurl4-openssl-dev openssl \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://mran.revolutionanalytics.com/install/mro/3.2.5/MRO-3.2.5-Ubuntu-14.4.x86_64.deb && \
    echo "f73378c5942621f8f49f7c28ce107608bfe71f808ba5bf58379437b2963b6894 MRO-3.2.5-Ubuntu-14.4.x86_64.deb" | sha256sum -c - && \
    dpkg -i MRO-3.2.5-Ubuntu-14.4.x86_64.deb && \
    rm MRO-3.2.5-Ubuntu-14.4.x86_64.deb



# Neovim
RUN add-apt-repository ppa:neovim-ppa/unstable && \
	apt-get update && apt-get install -yq neovim xclip xsel tmux && \
	apt-get install -yq python-dev python-pip python3-dev python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

# Configure environment
ENV SHELL /bin/zsh
ENV NB_USER diogro
ENV NB_UID 1000
ENV HOME /home/diogro
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Create diogro user with UID=1000 and in the 'users' group
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER 

USER diogro

# Setup diogro home directory
RUN mkdir /home/$NB_USER/projects
WORKDIR /home/$NB_USER
RUN curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
RUN sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
WORKDIR /home/$NB_USER/.spf13-vim-3
RUN git remote add personal https://github.com/diogro/spf13-vim.git
RUN git pull personal local:local && git checkout local
WORKDIR /home/$NB_USER/
RUN mkdir .config/ && mkdir .config/nvim
WORKDIR /home/$NB_USER/.spf13-vim-3
RUN make
RUN vim +BundleInstall +q +q

USER root

# R packages
# Install MCMCglmm
RUN R -e 'install.packages("ape", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("MCMCglmm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("httr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("git2r", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("devtools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("doMC", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("testthat", repos="http://cran.us.r-project.org")'

# Install evolqg dependencies
RUN R -e 'source("http://bioconductor.org/biocLite.R"); biocLite(c("graph", "Rgraphviz", "Biostrings"))'

RUN R -e 'install.packages("igraph", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("Matrix", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("vegan", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("ape", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("phytools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("mvtnorm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("coda", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("MCMCpack", repos="http://cran.us.r-project.org")'

# rgl dependencies
RUN apt-get update && apt-get install -yq --no-install-recommends \
	libx11-dev libgl1-mesa-dev libglu1-mesa-dev r-cran-rgl \
	&& apt-get purge r-base-core \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*


RUN R -e 'install.packages("rgl", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("geomorph", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("install.load", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("readr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("lme4", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("cowplot", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("readr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("tidyr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("dplyr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("evolqg", repos="http://cran.us.r-project.org")'


# Configure container startup as root
EXPOSE 8888
WORKDIR /home/$NB_USER/
ENTRYPOINT ["tini", "--"]

# Switch back to diogro to avoid accidental container runs as root
USER diogro
