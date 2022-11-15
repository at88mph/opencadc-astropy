FROM python:3.10-slim as builder

ENV ENV="PROD"

ARG CFITSIO_VERSION=4.1.0
ARG CFITSIO_URL=https://heasarc.gsfc.nasa.gov/FTP/software/fitsio/c/cfitsio-${CFITSIO_VERSION}.tar.gz
ARG FITSVERIFY_VERSION=4.20
ARG FITSVERIFY_URL=https://heasarc.gsfc.nasa.gov/docs/software/ftools/fitsverify/fitsverify-${FITSVERIFY_VERSION}.tar.gz

RUN apt-get update \
    && apt-get install -y autoconf gcc libc-dev libffi-dev libmagic-dev libnsl-dev libssl-dev libtool libxslt-dev make zlib1g-dev \
    && apt-get clean \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /src

ADD ${CFITSIO_URL} /src/
ADD ${FITSVERIFY_URL} /src/

RUN cd /src \
  && tar xvf cfitsio-${CFITSIO_VERSION}.tar.gz \
  && cd cfitsio-${CFITSIO_VERSION} \
  && ./configure --prefix=/usr/local  \
  && make \
  && make install

RUN cd /src \
  && tar xvf fitsverify-${FITSVERIFY_VERSION}.tar.gz \
  && cd fitsverify-${FITSVERIFY_VERSION} \
  && gcc -o fitsverify ftverify.c fvrf_data.c fvrf_file.c fvrf_head.c fvrf_key.c fvrf_misc.c -DSTANDALONE -I/usr/local/include -L/usr/local/lib -lcfitsio -lm -lnsl \
  && cp ./fitsverify /usr/local/bin/


FROM python:3.10-slim

# Upgrade pip to the latest.
RUN pip install --upgrade pip
RUN pip install --no-cache-dir "astropy>=5.0,<6.0" \
    && pip install --no-cache-dir "regions"

COPY --from=builder /usr/local/ /usr/local/

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/local.conf \
    ; ldconfig

WORKDIR /usr/src/app
ENTRYPOINT ["/entrypoint.sh"]
