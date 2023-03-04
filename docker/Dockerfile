# This Dockerfile sets up a NTField environment with PyTorch (GPU).

FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu18.04

ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && \
    apt install -y \
        wget git net-tools vim curl build-essential x11vnc zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev \
        libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev freeglut3-dev && \
        # following is for opencv-python (uncomment if not needed)
        apt install -y --no-install-recommends libglib2.0-0 libxrender1 libxext6 libsm6 libgl1-mesa-glx && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /ntf-wkdir
# copy files to install packages
COPY NTFields_env.yml /ntf-wkdir/NTFields_env.yml
COPY bvh-distance-queries /ntf-wkdir/bvh-distance-queries

ENV PATH="/root/miniconda3/bin:${PATH}"
ARG TORCH_CUDA_ARCH_LIST="8.6+PTX"
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    mkdir /root/.conda && \
    bash Miniconda3-latest-Linux-x86_64.sh -b && \
    rm -f Miniconda3-latest-Linux-x86_64.sh && \
    echo "Running $(conda --version)" && \
    conda init bash && . /root/.bashrc && conda update conda && \
    conda env create -f NTFields_env.yml

SHELL ["/bin/bash", "-c"]
RUN echo "source activate NTFields" >> ~/.bashrc && \
    export LIBGL_ALWAYS_INDIRECT=0 && \
    source /root/.bashrc
