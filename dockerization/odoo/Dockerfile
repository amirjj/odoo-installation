# Use an official Python runtime as a parent image
FROM python:3.11-slim

# Set environment variables
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Install required packages and dependencies
RUN apt-get update && apt-get install -y --fix-missing \
    git \
    build-essential \
    wget \
    python3-dev \
    libfreetype6-dev \
    libxml2-dev \
    libzip-dev \
    libldap2-dev \
    libsasl2-dev \
    node-less \
    libjpeg62-turbo-dev \
    zlib1g-dev \
    libpq-dev \
    libxslt1-dev \
    libtiff5-dev \
    libopenjp2-7-dev \
    liblcms2-dev \
    libwebp-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libxcb1-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create Odoo user and group
RUN groupadd -r odoo17 && useradd -m -d /opt/odoo17 -g odoo17 -s /bin/bash odoo17

# Switch to Odoo user
USER odoo17

# Set working directory
WORKDIR /opt/odoo17

# Clone Odoo source code
RUN git clone --depth 1 --branch 17.0 https://www.github.com/odoo/odoo /opt/odoo17/odoo

# Create and activate a virtual environment, then install dependencies
RUN python3 -m venv odoo-venv && \
    /bin/bash -c "source odoo-venv/bin/activate && pip install wheel && pip install -r /opt/odoo17/odoo/requirements.txt"

# Create custom addons directory
RUN mkdir /opt/odoo17/odoo-custom-addons

# Copy Odoo configuration file
COPY odoo17.conf /etc/odoo17.conf

# Expose Odoo port
EXPOSE 8069

# Define the command to run the Odoo server
CMD ["/opt/odoo17/odoo-venv/bin/python3", "/opt/odoo17/odoo/odoo-bin", "-c", "/etc/odoo17.conf"]
