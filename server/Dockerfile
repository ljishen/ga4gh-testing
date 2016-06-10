# VERSION 1.0

FROM ubuntu:latest 

MAINTAINER Jianshen Liu <jliu120@ucsc.edu>

# Install basic pre-requisite packages
RUN apt-get update \
    && apt-get install -y \
        python-dev \
        python-pip \
        zlib1g-dev \
        libxslt1-dev \
        libffi-dev \
        libssl-dev \
        apache2 \
        libapache2-mod-wsgi \
        git

# Create the Python egg cache directory,
# and make it writable by the www-data user
RUN mkdir /var/cache/apache2/python-egg-cache \
    && chown www-data:www-data /var/cache/apache2/python-egg-cache/

# Clone the source code
RUN git clone https://github.com/ga4gh/server.git /srv/ga4gh/server

WORKDIR /srv/ga4gh/server

# Configure the python requirements as a separate step
# prior to the build so that changes to the GA4GH Server
# codebase do not trigger a full rebuild of the pip requirements.
RUN pip install -r requirements.txt

# Install server from source
RUN python setup.py install

# Write configuration files
COPY application.wsgi /srv/ga4gh/application.wsgi
COPY config.py /srv/ga4gh/config.py
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 80

# Start server when container starts
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
