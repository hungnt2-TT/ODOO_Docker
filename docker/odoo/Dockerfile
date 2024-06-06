FROM ubuntu:jammy

ENV LANG en_US.UTF-8

# Install dependencies
ENV DEBIAN_FRONTEND=noninteractive
COPY ./src/debian/control /tmp/control
RUN apt update && \
    sed -n -e '/^Depends:/,/^Pre/ s/ python3-\(.*\),/python3-\1/p' /tmp/control | xargs apt-get install --no-install-recommends -y
RUN apt update && \
    apt install --no-install-recommends -y npm && \
    npm install -g rtlcss
RUN apt update && \
    apt install --no-install-recommends -y \
    python3-venv \
    python3-dev \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev \
    libsasl2-dev \
    libldap2-dev \
    build-essential \
    libssl-dev \
    libffi-dev \
    libmysqlclient-dev \
    libjpeg-dev \
    libpq-dev \
    libjpeg8-dev \
    liblcms2-dev \
    libblas-dev \
    libatlas-base-dev

# Create the user
RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1001 ubuntu
WORKDIR /home/ubuntu/app
USER ubuntu

# Create venv
RUN python3 -m venv /home/ubuntu/venv
ENV PATH="/home/ubuntu/venv/bin:$PATH"
ENV PYTHONDONTWRITEBYTECODE=1

# Install python dependencies
COPY ./src/requirements.txt .
RUN pip install -r requirements.txt
RUN pip install inotify

COPY ./src .
CMD [ "python", "odoo-bin", "--dev=all", "--config", "/home/ubuntu/etc/odoo.conf"]