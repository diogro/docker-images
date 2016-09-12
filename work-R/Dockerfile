# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM jupyter/r-notebook

MAINTAINER Diogo Melo <diogro@gmail.com>

# Now switch to jovyan for all conda and other package manager installs
USER jovyan

RUN R -e 'install.packages("MCMCglmm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("car", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("vegan", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("ape", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("phytools", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("mvtnorm", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("install.load", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("readr", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("lme4", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("cowplot", repos="http://cran.us.r-project.org")'
RUN R -e 'install.packages("evolqg", repos="http://cran.us.r-project.org")'
