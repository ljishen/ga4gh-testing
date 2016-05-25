# VERSION 1.0

# Support Tini
FROM krallin/ubuntu-tini:trusty

MAINTAINER Jianshen Liu <jliu120@ucsc.edu>

# Install basic pre-requisite packages
RUN apt-get update \
    && apt-get install -y \
        wget \
        libffi-dev \
        zlib1g-dev \
        libxslt1-dev \
        libcurl4-openssl-dev \
        git

# Install Miniconda
RUN wget -q https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh \
    && /bin/bash Miniconda2-latest-Linux-x86_64.sh -b -p /opt/miniconda2 \
    && rm Miniconda2-latest-Linux-x86_64.sh

ENV PATH /opt/miniconda2/bin:$PATH

# Install jupyter kernel gateway
RUN conda install --quiet --yes \
    jupyter \
    && conda clean -ay

# Clone the source code
RUN git clone https://github.com/ga4gh/server.git /srv/ga4gh/server

WORKDIR /srv/ga4gh/server

# Configure the python requirements as a separate step
# prior to the build so that changes to the GA4GH Server
# codebase do not trigger a full rebuild of the pip requirements.
RUN pip install -r requirements.txt

# Install server from source
RUN python setup.py install

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure container startup
WORKDIR /home
EXPOSE 8888
ENTRYPOINT ["tini", "--", "jupyter", "notebook"]

# Add local files as late as possible to avoid cache busting
COPY jupyter_notebook_config.py /root/.jupyter/
