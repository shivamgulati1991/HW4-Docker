FROM ubuntu:latest

RUN apt-get update && \
      apt-get -y install sudo
RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo
RUN sudo apt-get install -y socat
RUN sudo apt-get install -y curl
