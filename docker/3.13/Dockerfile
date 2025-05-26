FROM python:3.13-slim as builder

ENV ENV="PROD"

ARG FITSVERIFY_VERSION=4.20
ARG FITSVERIFY_URL=https://heasarc.gsfc.nasa.gov/docs/software/ftools/fitsverify/fitsverify-${FITSVERIFY_VERSION}.tar.gz

RUN apt-get update \
    && apt-get install -y autoconf gcc libc-dev libcfitsio-dev libffi-dev libmagic-dev libnsl-dev libssl-dev libtool libxslt-dev make zlib1g-dev \
    && mkdir -p /src

ADD ${FITSVERIFY_URL} /src/

RUN cd /src \
  && tar xvf fitsverify-${FITSVERIFY_VERSION}.tar.gz \
  && cd fitsverify-${FITSVERIFY_VERSION} \
  && gcc -o fitsverify ftverify.c fvrf_data.c fvrf_file.c fvrf_head.c fvrf_key.c fvrf_misc.c -DSTANDALONE -I/usr/local/include -L/usr/local/lib -lcfitsio -lm -lnsl \
  && cp ./fitsverify /usr/local/bin/

# Upgrade pip to the latest.
RUN pip install --upgrade pip
RUN pip install --no-cache-dir "astropy" \
    && pip install --no-cache-dir "regions"


FROM python:3.13-slim as libs

RUN apt-get update \
    && apt-get install -y libcfitsio-bin libmagic-dev


FROM python:3.13-slim

COPY --from=libs /usr/ /usr/
COPY --from=libs /etc/ /etc/
COPY --from=builder /usr/local/ /usr/local/

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/local.conf \
    ; ldconfig

WORKDIR /usr/src/app
ENTRYPOINT ["/entrypoint.sh"]
