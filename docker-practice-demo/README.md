# Docker SSH Setup

## Overview

This Docker setup creates a container with a preinstalled SSH server and some utility packages. The SSH server is configured for key-based authentication, and a named volume is used to persist the user's home directory across container runs.

## Dockerfile

```Dockerfile
# Use a lightweight base image
FROM debian:latest

# Install necessary packages
RUN apt-get update && apt-get install -y \
    htop \
    mc \
    openssh-server \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Add a user named "de" with sudo privileges
RUN useradd -m -s /bin/bash de \
    && echo 'de:hirwa@12345' | chpasswd \
    && adduser de sudo

# Configure SSH server
RUN mkdir /var/run/sshd \
    && echo 'PermitRootLogin no' >> /etc/ssh/sshd_config \
    && echo 'PasswordAuthentication no' >> /etc/ssh/sshd_config \
    && echo 'AllowUsers de' >> /etc/ssh/sshd_config

# Expose port 22
EXPOSE 22

# Set up SSH key-based authentication
RUN mkdir -p /home/de/.ssh \
    && chmod 700 /home/de/.ssh

# Define the volume for the home directory
VOLUME ["/home/de"]

# Start SSH service
CMD ["/usr/sbin/sshd", "-D"]
