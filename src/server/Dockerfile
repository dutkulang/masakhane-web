# base image
FROM python:3.11-slim

# set working directory 
WORKDIR /usr/src/app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install system dependencies
# Recommended for Dockerfile
RUN apt-get update -y && apt-get install -y netcat-openbsd
RUN apt update -y
RUN apt-get update -y && \
    apt-get install -y \
        wget \
        gnupg \
        lsb-release \
        ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Get the LSB release codename
RUN lsb_release -c -s > /tmp/lsb_release

# Add GCSFuse repository with the modern key method
RUN GCSFUSE_REPO=$(cat /tmp/lsb_release) && \
    echo "deb [signed-by=/usr/share/keyrings/gcsfuse-archive-keyring.gpg] http://packages.cloud.google.com/apt gcsfuse-$GCSFUSE_REPO main" | tee /etc/apt/sources.list.d/gcsfuse.list && \
    wget -O /usr/share/keyrings/gcsfuse-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Update and install
RUN apt-get update -y && \
    apt-get install -y gcsfuse && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get install -y gcsfuse


# add and 
COPY ./requirements.txt /usr/src/app/requirements.txt
# RUN pip install to install requirements 
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# add entrypoint.sh
COPY ./entrypoint.sh /usr/src/app/entrypoint.sh

# add app
COPY . /usr/src/app

# run server (https://github.com/testdrivenio/testdriven-app/issues/25)
CMD ["sh","-c","chmod 777 /usr/src/app/entrypoint.sh"]
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
