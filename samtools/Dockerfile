# VERSION 1.0

FROM ubuntu:latest

MAINTAINER Jianshen Liu <jliu120@ucsc.edu>

RUN apt-get update \
    && apt-get install -y \
        build-essential \
        zlib1g-dev \
        libncurses5-dev \
        wget

ENV SAMTOOLS_VERSION 1.3.1

WORKDIR /root
RUN wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
    && tar -xf samtools-${SAMTOOLS_VERSION}.tar.bz2 \
    && rm samtools-${SAMTOOLS_VERSION}.tar.bz2

RUN wget https://github.com/samtools/htslib/releases/download/1.3.1/htslib-${SAMTOOLS_VERSION}.tar.bz2 \
    && tar -xf htslib-${SAMTOOLS_VERSION}.tar.bz2 \
    && rm htslib-${SAMTOOLS_VERSION}.tar.bz2

WORKDIR /root/htslib-${SAMTOOLS_VERSION}
RUN make && make install

WORKDIR /root/samtools-${SAMTOOLS_VERSION}
RUN ./configure --with-htslib=/usr/local/include/htslib \
    && make \
    && make install

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
