FROM blacktop/volatility:2.6

LABEL maintainer "https://github.com/blacktop"

ENV CUCKOO_VERSION 2.0.4
ENV CUCKOO_CWD /cuckoo
ENV SSDEEP ssdeep-2.13

# Install Cuckoo Sandbox Required Dependencies
COPY requirements.txt /tmp/requirements.txt
RUN apk add --no-cache tcpdump py-lxml py-chardet py-libvirt py-crypto curl
RUN apk update && apk add --no-cache postgresql-dev \
                       gcc \
                       g++ \
                       python-dev \
                       libpq \
                       py-pip \
  && pip install --upgrade pip wheel \
  && pip install psycopg2 \
  && apk del --purge postgresql-dev \
                     gcc \
                     g++
RUN apk add --no-cache -t .build-deps \
                           autoconf \
                           automake \
                           build-base \
                           file-dev \
                           git \
                           jpeg-dev \
                           libc-dev \
                           libffi-dev \
                           libstdc++ \
                           libtool \
                           libxml2-dev \
                           libxslt-dev \
                           linux-headers \
                           openssl-dev \
                           py-pip \
                           python-dev \
                           zlib-dev \
  && set -x \
  && echo "===> Install ssdeep..." \
  && wget -O /tmp/$SSDEEP.tar.gz https://downloads.sourceforge.net/project/ssdeep/$SSDEEP/$SSDEEP.tar.gz \
  && cd /tmp \
  && tar zxvf $SSDEEP.tar.gz \
  && cd $SSDEEP \
  && ./configure \
  && make \
  && make install \
  && echo "===> Install pydeep..." \
  && cd /tmp \
  && git clone https://github.com/kbandla/pydeep.git \
  && cd pydeep \
  && python setup.py build \
  && python setup.py install \
  && echo "===> Install Cuckoo Sandbox..." \
  && mkdir /cuckoo \
  && adduser -D -h /cuckoo cuckoo \
  && export PIP_NO_CACHE_DIR=off \
  && export PIP_DISABLE_PIP_VERSION_CHECK=on \
  && pip install --upgrade pip wheel \
  && LDFLAGS=-L/lib pip install cuckoo==$CUCKOO_VERSION \
  && cuckoo \
  && cuckoo community \
  && echo "===> Install mitmproxy..." \
  && LDFLAGS=-L/lib pip install mitmproxy \
  && pip install -r /tmp/requirements.txt \
  && echo "===> Clean up unnecessary files..." \
  && rm -rf /tmp/* \
  && apk del --purge .build-deps

COPY conf /cuckoo/conf
COPY update_conf.py /update_conf.py
COPY docker-entrypoint.sh /entrypoint.sh

WORKDIR /cuckoo

VOLUME ["/cuckoo/conf"]

EXPOSE 1337 31337

ENTRYPOINT ["/entrypoint.sh"]
CMD ["--help"]
