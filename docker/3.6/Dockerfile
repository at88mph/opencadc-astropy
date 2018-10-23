FROM python:3.6-alpine

WORKDIR /usr/src/app

COPY requirements.txt ./

RUN apk --no-cache add gcc libc-dev libffi-dev openssl-dev bsd-compat-headers libxslt-dev

RUN pip install --no-cache-dir -r requirements.txt
