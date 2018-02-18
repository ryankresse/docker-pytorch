FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04 
RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         wget \
         curl \
         vim \
         ca-certificates \         
         libjpeg-dev \
         software-properties-common \
         libpng-dev &&\
     rm -rf /var/lib/apt/lists/*

#Install apt-add
RUN apt-get install software-properties-common

#Install conda
ENV PYTHON_VERSION=3.6
RUN curl -o ~/miniconda.sh -O  https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh  && \
     chmod +x ~/miniconda.sh && \
     ~/miniconda.sh -b -p /opt/conda && \     
     rm ~/miniconda.sh && \
    /opt/conda/bin/conda install conda-build && \
     /opt/conda/bin/conda clean -ya 

#Install libraries
ENV PATH /opt/conda/bin:$PATH
RUN conda install matplotlib=2.0* seaborn numpy scipy scikit-learn jupyter pandas 
RUN conda install pytorch torchvision cuda90 -c pytorch
RUN pip install sklearn-pandas

#TINI
RUN wget --quiet https://github.com/krallin/tini/releases/download/v0.10.0/tini && \
    echo "1361527f39190a7338a0b434bd8c88ff7233ce7b9a4876f3315c22fce7eca1b0 *tini" | sha256sum -c - && \
    mv tini /usr/local/bin/tini && \
    chmod +x /usr/local/bin/tini

EXPOSE 8888

ENTRYPOINT ["tini", "--"]

CMD jupyter notebook --notebook-dir=/workspace --ip='*' --port=8888 --no-browser --allow-root --NotebookApp.token=


