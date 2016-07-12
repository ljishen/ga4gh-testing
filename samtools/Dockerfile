# VERSION 1.0

FROM ubuntu:latest

MAINTAINER Jianshen Liu <jliu120@ucsc.edu>

# "bzip2" for decompress `.tar.bz2` file
# "libncurses5-dev" and "zlib1g-dev" for "./configure"
RUN apt-get update \
    && apt-get install -y \
        build-essential \
        bzip2 \
        curl \
        zlib1g-dev \
        libncurses5-dev

ENV SAMTOOLS_VERSION 1.3.1

WORKDIR /root
RUN mkdir samtools \
    && curl -fsSL https://github.com/samtools/samtools/releases/download/$SAMTOOLS_VERSION/samtools-$SAMTOOLS_VERSION.tar.bz2 \
        | tar -jxC samtools --strip-components=1

WORKDIR /root/samtools
RUN ./configure \
    && make all all-htslib \
    && make install install-htslib

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["samtools"]
