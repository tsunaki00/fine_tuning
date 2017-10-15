FROM nvidia/cuda:8.0-cudnn6-runtime-ubuntu16.04

MAINTAINER tsunaki@gauss-ai.jp

# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        wget \
        libfreetype6-dev \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libjasper-dev \
        libavformat-dev \
        libpq-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python3 \
        python3-dev \
        rsync \
        software-properties-common \
        unzip \
        libmysqlclient-dev \        
        cmake \
        git \
        unzip \
        yasm \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python3 get-pip.py && \
    rm get-pip.py

RUN pip3 --no-cache-dir install \
        Pillow \
        h5py \
        ipykernel \
        jupyter \
        matplotlib \
        numpy \
        pandas \
        scipy \
        sklearn \
        ipywidgets \
        && \
    python3 -m ipykernel.kernelspec

# Install TensorFlow GPU version.
RUN pip3 --no-cache-dir --default-timeout=1000 install --upgrade tensorflow-gpu
RUN pip3 --no-cache-dir install keras

## jupyter_notebook
COPY scripts/jupyter_notebook_config.py /root/.jupyter/
COPY scripts/run_jupyter.sh /
RUN chmod -R 755 /root/.jupyter/jupyter_notebook_config.py
RUN chmod -R 755 /run_jupyter.sh
# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH


# IPython
EXPOSE 8888


WORKDIR "/notebooks"
CMD ["/run_jupyter.sh", "--allow-root"]
