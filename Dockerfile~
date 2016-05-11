FROM nvidia/cuda:7.0-cudnn4-devel
ENV CAFFE_VERSION 0.14
ENV RR_CONTAINER hmlatapie/rr_caffe
LABEL com.nvidia.caffe.version="0.14"
ENV CAFFE_PKG_VERSION 0.14.2-1

#install basic environment
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y aptitude
RUN apt-get install -y ipython
RUN apt-get install -y python-pip
RUN apt-get install -y man
RUN apt-get install -y vim
RUN apt-get install -y git

#install nvidia's flavor of caffe from source
RUN cd /root \
	&& git clone https://github.com/NVIDIA/caffe.git \
	&& cd caffe \
	&& for req in $(cat python/requirements.txt) pydot; do pip install $req; done \
	&& mkdir build \
	&& cd build \
	&& cmake -DUSE_CUDNN=1 .. \
 	&& make -j"$(nproc)"

#install apollo caffe
RUN cd /root \
	&& git clone http://github.com/Russell91/apollocaffe.git \
	&& cd apollocaffe \
	&& for req in $(cat python/requirements.txt); do pip install $req; done 

#install mongo
RUN pip install pymongo

#install sacred
RUN cd /root && git clone https://github.com/IDSIA/sacred.git && cd sacred && python setup.py install

RUN apt-get install -y --no-install-recommends --force-yes \
            caffe-nv=$CAFFE_PKG_VERSION \
            caffe-nv-tools=$CAFFE_PKG_VERSION \
            python-caffe-nv=$CAFFE_PKG_VERSION && \
    rm -rf /var/lib/apt/lists/*

VOLUME /root/rr
WORKDIR /root

CMD /bin/bash

