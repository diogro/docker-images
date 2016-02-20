# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM diogro/work-r

MAINTAINER Diogo Melo <diogro@gmail.com>

# Now switch to jovyan for all conda and other package manager installs
USER jovyan

# Python packages
RUN conda install --yes numpy pandas scikit-learn scikit-image matplotlib scipy seaborn sympy cython patsy statsmodels cloudpickle dill numba bokeh && conda clean -yt

# Now for a python2 environment
RUN conda create -p $CONDA_DIR/envs/python2 python=2.7 ipykernel numpy pandas scikit-learn scikit-image matplotlib scipy seaborn sympy cython patsy statsmodels cloudpickle dill numba bokeh && conda clean -yt
RUN $CONDA_DIR/envs/python2/bin/python \
    $CONDA_DIR/envs/python2/bin/ipython \
    kernelspec install-self --user
RUN /bin/bash -c "source activate $CONDA_DIR/envs/python2 && pip install dendropy"

# R packages
RUN conda config --add channels r
RUN conda install --yes r-irkernel r-plyr r-devtools r-rcurl r-dplyr r-ggplot2 r-caret rpy2 r-tidyr r-shiny r-rmarkdown r-forecast r-stringr r-rsqlite r-reshape2 r-nycflights13 r-randomforest && conda clean -yt

# Clone featured notebooks before adding local content to avoid recloning
# everytime something changes locally
RUN mkdir -p /home/jovyan/work/Bio507
RUN git clone --depth 1 https://github.com/diogro/dadosBioEvol /home/jovyan/work/Bio507/

USER root

# Convert notebooks to the current format and trust them
RUN find /home/jovyan/work -name '*.ipynb' -exec jupyter nbconvert --to notebook {} --output {} \; && \
    chown -R jovyan:users /home/jovyan && \
    sudo -u jovyan env "PATH=$PATH" find /home/jovyan/work -name '*.ipynb' -exec jupyter trust {} \;
