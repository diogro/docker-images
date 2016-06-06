# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM jupyter/r-notebook

MAINTAINER Diogo Melo <diogro@gmail.com>

# Install system libraries first as root
USER root

# R dependencies that conda can't provide (X, fonts, compilers)
RUN apt-get update && \
    apt-get install -y libxrender1 fonts-dejavu gfortran gcc mesa-common-dev libglu1-mesa-dev libgl1-mesa-dev && \
    apt-get install -y software-properties-common && \
    apt-get clean

# neovim install
RUN apt-get update && \
    apt-get install -y neovim zsh curl && \
    apt-get clean

# Now switch to jovyan for all conda and other package manager installs
USER jovyan

# Install MCMCglmm
RUN R -e 'install.packages("MCMCglmm", repos="http://cran.us.r-project.org")'

# Install evolqg dependencies
RUN R -e 'source("http://bioconductor.org/biocLite.R"); biocLite(c("graph", "Rgraphviz", "Biostrings"))'

RUN conda install --yes r-rgl && conda clean -yt

RUN R -e 'install.packages("igraph", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("Matrix", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("vegan", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("ape", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("phytools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("mvtnorm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("coda", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("MCMCpack", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("geomorph", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("install.load", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("readr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("lme4", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("cowplot", repos="http://cran.us.r-project.org")'

RUN curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh

# Install evolqg from source
COPY evolqg_0.2-5.tar.gz /home/jovyan
RUN R -e 'install.packages("/home/jovyan/evolqg_0.2-5.tar.gz", repos=NULL, type = "source")'
