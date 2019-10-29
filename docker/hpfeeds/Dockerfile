### This is only for testing purposes, do NOT use for production
FROM alpine

ADD dist/ /root/dist/

# Install packages
RUN apk -U --no-cache add \
               build-base \
               coreutils \
               git \
               libffi \
               libffi-dev \
               py-gevent \
               py-pip \
               python \
               python-dev \
               sqlite && \
    git clone --depth=1 https://github.com/rep/hpfeeds /opt/hpfeeds && \
    cd /opt/hpfeeds/hpfeeds/broker && \
    sed -i -e '87d;88d' database.py && \
    cd /opt/hpfeeds/hpfeeds/broker && timeout 5 python server.py || : && \
    sqlite3 db.sqlite3 < /root/dist/adduser.sql

RUN apk del --purge autoconf \
                    build-base \
                    coreutils \
                    libffi-dev \
                    python-dev && \
    rm -rf /root/* && \
    rm -rf /var/cache/apk/*

WORKDIR /opt/hpfeeds/broker
CMD python server.py
