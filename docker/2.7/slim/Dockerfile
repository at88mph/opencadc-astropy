FROM python:2.7-slim

WORKDIR /usr/src/app
ENV ENV="PROD"

COPY entrypoint.sh /entrypoint.sh

RUN apt-get update \
    && apt-get install -y gcc libc-dev libffi-dev libmagic-dev libssl-dev libxslt-dev make \
    && apt-get clean \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip to the latest.
RUN pip install --upgrade pip
RUN pip install --no-cache-dir "astropy>=2.0.16,<4.0" \
    && pip install --no-cache-dir "regions"

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
