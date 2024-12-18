# Use NVIDIA CUDA 11.3 base image
FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON_VERSION=3.7.15
ENV TORCH_VERSION=1.10.1
ENV TORCHVISION_VERSION=0.11.2
ENV TORCHAUDIO_VERSION=0.10.1
ENV CUDA_TOOLKIT=11.3

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
    tk-dev \
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python 3.7.15
RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz \
    && tar -xzf Python-${PYTHON_VERSION}.tgz \
    && cd Python-${PYTHON_VERSION} \
    && ./configure --enable-optimizations --with-ssl \
    && make -j$(nproc) \
    && make altinstall \
    && cd .. \
    && rm -rf Python-${PYTHON_VERSION} Python-${PYTHON_VERSION}.tgz

# Use Python 3.7 as default
RUN ln -sf /usr/local/bin/python3.7 /usr/bin/python3 \
    && ln -sf /usr/local/bin/pip3.7 /usr/bin/pip3

# Upgrade pip
RUN python3.7 -m pip install --upgrade pip setuptools

# Install PyTorch, Torchvision, Torchaudio with CUDA 11.3
RUN pip3 install torch==${TORCH_VERSION}+cu113 \
    torchvision==${TORCHVISION_VERSION}+cu113 \
    torchaudio==${TORCHAUDIO_VERSION} \
    -f https://download.pytorch.org/whl/cu113/torch_stable.html

# Install Jupyter Notebook
RUN pip3 install jupyter notebook

# Create a workspace directory
WORKDIR /workspace

# Expose port for Jupyter Notebook
EXPOSE 8888

# Run Jupyter Notebook on container startup
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
