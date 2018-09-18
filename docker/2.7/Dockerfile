FROM python:2.7-alpine

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN apk --no-cache add gcc libc-dev libffi-dev openssl-dev \
  && pip install --no-cache-dir -r requirements.txt
