# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM krallin/ubuntu-tini:trusty

MAINTAINER Diogo Melo <diogro@gmail.com>

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && apt-get install -yq --no-install-recommends \
    git \
    vim \
    jed \
    emacs \
    build-essential \
    python-dev \
    unzip \
    libsm6 \
    pandoc \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    zsh \
    curl \
    software-properties-common \
    texlive-latex-base \
    texlive-latex-extra \
    texlive-fonts-extra \
    texlive-fonts-recommended \
    texlive-generic-recommended \
    libxrender1 \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# R dependencies
RUN apt-get update && apt-get install -yq --no-install-recommends \
    libcairo2 libgfortran3 libglib2.0-0 libjpeg8 libpango-1.0-0 libpangocairo-1.0-0 \
    libtcl8.6 libtiff5 libtk8.6 libx11-6 libxt6 gfortran fonts-dejavu libblas-dev liblapack-dev\
    libx11-dev mesa-common-dev libglu1-mesa-dev libcurl4-openssl-dev openssl \
    libx11-dev libgl1-mesa-dev libglu1-mesa-dev \
    libpaper-utils libpaper1 libtcl8.5 libtk8.5 tcl8.5 tk8.5 unzip xdg-utils zip ed \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Neovim
RUN add-apt-repository ppa:neovim-ppa/unstable && \
    apt-get update && apt-get install -yq neovim xclip xsel tmux && \
    apt-get install -yq python-dev python-pip python3-dev python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

# Configure environment
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH
ENV SHELL /bin/zsh
ENV NB_USER diogro
ENV NB_UID 1000
ENV HOME /home/diogro
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Create diogro user with UID=1000 and in the 'users' group
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir -p /opt/conda && \
    chown diogro /opt/conda

USER diogro

# Setup diogro home directory
RUN mkdir /home/$NB_USER/projects && \
    mkdir /home/$NB_USER/.jupyter && \
    mkdir -p -m 700 /home/$NB_USER/.local/share/jupyter && \
    echo "cacert=/etc/ssl/certs/ca-certificates.crt" > /home/$NB_USER/.curlrc

# Install conda as diogro
RUN cd /tmp && \
    mkdir -p $CONDA_DIR && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-3.19.0-Linux-x86_64.sh && \
    echo "9ea57c0fdf481acf89d816184f969b04bc44dea27b258c4e86b1e3a25ff26aa0 *Miniconda3-3.19.0-Linux-x86_64.sh" | sha256sum -c - && \
    /bin/bash Miniconda3-3.19.0-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-3.19.0-Linux-x86_64.sh && \
    $CONDA_DIR/bin/conda install --quiet --yes conda==3.19.1 && \
    $CONDA_DIR/bin/conda config --system --add channels conda-forge && \
    conda clean -tipsy

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

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list
RUN gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
RUN gpg -a --export E084DAB9 | apt-key add -

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    r-base \
    r-cran-rgl \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# R packages
RUN R -e 'install.packages("devtools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("dplyr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("tidyr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("rmarkdown", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("igraph", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("testthat", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("lme4", repos="http://cran.us.r-project.org")'
RUN R -e 'source("http://bioconductor.org/biocLite.R"); biocLite(c("Biostrings"))'
RUN R -e 'install.packages("MCMCglmm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("phytools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("MCMCpack", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("install.load", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("readr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("cowplot", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("evolqg", repos="http://cran.us.r-project.org")'

# vim
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libncurses5-dev \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt
RUN git clone https://github.com/vim/vim.git
WORKDIR vim/src
RUN make && make install

USER diogro

RUN git config --global user.email "diogro@gmail.com"
RUN git config --global user.name "Diogo Melo"

WORKDIR /home/$NB_USER/.oh-my-zsh
RUN git remote add personal https://github.com/diogro/oh-my-zsh.git
RUN git pull personal local:local && git checkout local

USER root

# Configure container startup as root
EXPOSE 8888
WORKDIR /home/$NB_USER/
ENTRYPOINT ["tini", "--"]

# Switch back to diogro to avoid accidental container runs as root
USER diogro
