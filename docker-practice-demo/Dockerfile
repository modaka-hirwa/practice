# Start with a base image
FROM debian:latest

# Install necessary packages
RUN apt-get update && apt-get install -y \
    htop \
    mc \
    openssh-server \
    nano \
    curl\
    sudo

# Create a new user with sudo privileges
RUN useradd -m -s /bin/bash de && echo 'de:here is for password' | chpasswd && usermod -aG sudo de

# Set up SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
RUN echo 'PubkeyAuthentication yes' >> /etc/ssh/sshd_config

# Expose the SSH port
EXPOSE 22

# Start the SSH service

RUN apt-get update && apt-get install -y openssh-server && \
    ssh-keygen -A

CMD ["/usr/sbin/sshd", "-D"]

