#!/bin/bash
# Use NVIDIA CUDA 11.3 base image
FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV SHELL=/bin/bash

ENV APT_INSTALL="apt-get install -y --no-install-recommends"
ENV PIP_INSTALL="python3 -m pip --no-cache-dir install --upgrade"
ENV GIT_CLONE="git clone --depth 10"

# Install system dependencies including SSL libraries
RUN apt-get update && \
    $APT_INSTALL \
    python3-dev \
    python3-pip \
    python3-venv \
    wget \
    curl \
    git \
    sudo \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libgdbm-dev \
    libdb5.3-dev \
    libbz2-dev \
    libexpat1-dev \
    liblzma-dev \
    libffi-dev \
    tk-dev \
    ffmpeg \
    libsm6 \
    libxext6 \
    ca-certificates \
    vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Miniforge
RUN curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh" \
    && bash Miniforge3-$(uname)-$(uname -m).sh -b \
    && echo "source /root/miniforge3/etc/profile.d/conda.sh" >> /root/.bashrc \
    && rm Miniforge3-$(uname)-$(uname -m).sh

# Install Jupyter Notebook
RUN $PIP_INSTALL jupyter notebook

# use a bash shell instead of an sh
SHELL ["/bin/bash", "-ec"]
RUN ln -sf /bin/bash /bin/sh

# Expose port for Jupyter Notebook
EXPOSE 8888 6006

CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True
