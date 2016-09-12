# Copyright (c) by Diogo Melo
# Distributed under the terms of the MIT License.

FROM diogro/work-r

MAINTAINER Diogo Melo <diogro@gmail.com>

# Now switch to jovyan for all conda and other package manager installs
USER jovyan

RUN mkdir -p /home/jovyan/QGcourse
RUN git clone https://github.com/diogro/QGcourse /home/jovyan/QGcourse

USER root

# Convert notebooks to the current format and trust them
RUN find /home/jovyan/work -name '*.ipynb' -exec jupyter nbconvert --to notebook {} --output {} \; && \
    chown -R jovyan:users /home/jovyan && \
    sudo -u jovyan env "PATH=$PATH" find /home/jovyan/work -name '*.ipynb' -exec jupyter trust {} \;

USER jovyan
