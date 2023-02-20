# BioGPT https://github.com/microsoft/BioGPT

FROM nvidia/cuda:11.8.0-runtime-ubuntu22.04
ENV APP_DIR=/app
WORKDIR ${APP_DIR}
RUN apt-get update
RUN apt-get install -y git wget
RUN apt-get install -y software-properties-common

# Python 3.10
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get -y install python3.10 python3.10-distutils python3-pip
RUN python3.10 -m pip install -U pip wheel setuptools

# Torch
RUN python3.10 -m pip install torch==1.13.1+cu116 -f https://download.pytorch.org/whl/torch_stable.html

# fairseq
RUN git clone https://github.com/pytorch/fairseq
RUN (cd fairseq; git checkout v0.12.2; pip install .; python3.10 setup.py build_ext --inplace)

# Moses
RUN git clone https://github.com/moses-smt/mosesdecoder.git
ENV MOSES=${APP_DIR}/mosesdecoder

# fastBPE
RUN git clone https://github.com/glample/fastBPE.git
ENV FASTBPE=${APP_DIR}/fastBPE
RUN (cd fastBPE; g++ -std=c++11 -pthread -O3 fastBPE/main.cc -IfastBPE -o fast; python3.10 setup.py build_ext --inplace; pip install .)

# sacremoses
RUN pip install sacremoses

# sklearn
RUN pip install scikit-learn

# BioGPT
RUN git clone https://github.com/microsoft/BioGPT.git

WORKDIR ${APP_DIR}/BioGPT
