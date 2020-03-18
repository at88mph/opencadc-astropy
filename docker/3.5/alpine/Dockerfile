FROM python:3.5-alpine

WORKDIR /usr/src/app
ENV ENV="PROD"

COPY entrypoint.sh /entrypoint.sh

RUN apk --no-cache add bsd-compat-headers gcc libc-dev libffi-dev libmagic libxslt-dev openssl-dev

# Upgrade pip to the latest.
RUN pip install --upgrade pip
RUN pip install --no-cache-dir astropy<4.0 \
    && pip install --no-cache-dir regions
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
