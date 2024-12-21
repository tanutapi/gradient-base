# Use NVIDIA CUDA 11.3 base image
FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive

# Set bash as the default shell
SHELL ["/usr/bin/bash", "-c"]

# Install system dependencies including SSL libraries
RUN apt-get update && apt-get install -y --no-install-recommends \
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

RUN curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
RUN bash Miniforge3-$(uname)-$(uname -m).sh -b
RUN source /root/miniforge3/etc/profile.d/conda.sh

# Install Jupyter Notebook
RUN pip3 install jupyter notebook

# Expose port for Jupyter Notebook
EXPOSE 8888

# Run Jupyter Notebook on container startup
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]