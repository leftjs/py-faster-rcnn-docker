FROM nvidia/cuda:7.5-cudnn3-devel

COPY files/.screenrc /root/.screenrc

#RUN apt-get update && apt-get install -y screen wget nano libgoogle-glog-dev liblmdb-dev git openssh-client libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler bundler
RUN apt-get update && apt-get install -y screen wget nano openssh-client protobuf-compiler bundler libopencv-dev

RUN wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh -O ~/anaconda3.sh
RUN bash ~/anaconda3.sh -b -p ~/anaconda3
ENV PATH /root/anaconda3/bin:${PATH}
COPY files/caffe-requirements-conda3.txt /tmp/caffe-requirements-conda3.txt
RUN conda create -y --name py-faster-r-cnn python=3.5 --file /tmp/caffe-requirements-conda3.txt -c conda-forge -c menpo -c ActivisionGameScience
RUN /bin/bash -c "source activate py-faster-r-cnn && pip install easydict"
RUN echo "source activate py-faster-r-cnn" >> /root/.bashrc
RUN rm ~/anaconda3.sh

WORKDIR /root/anaconda3/envs/py-faster-r-cnn
RUN  mkdir -p ./etc/conda/activate.d
RUN mkdir -p ./etc/conda/deactivate.d
RUN echo $'#!/bin/sh\n\
         export OLD_LD_LIBRARY_PATH=$LD_LIBRARY_PATH\n\
         export LD_LIBRARY_PATH=/root/anaconda3/envs/py-faster-r-cnn/lib:$LD_LIBRARY_PATH\n'\
         > ./etc/conda/activate.d/env_vars.sh
RUN echo $'#!/bin/sh\n\
          export LD_LIBRARY_PATH=$OLD_LD_LIBRARY_PATH\n\
          unset OLD_LD_LIBRARY_PATH\n'\
          > ./etc/conda/deactivate.d/env_vars.sh


# Installing lippng16
#RUN cd /root
#RUN git clone https://github.com/dimensio/libpng16-deb.git
#RUN cd libpng16-deb
#RUN bundle install
#RUN make

# For solving caffe compilation problem with float128 see:
# http://www.pcl-users.org/Installation-Trouble-Boost-float128-td4040519.html

# For solving caffe compilation problem with libpng16-deb see:
# http://stackoverflow.com/questions/31962975/caffe-install-on-ubuntu-for-anaconda-with-python-2-7-fails-with-libpng16-so-16-n

# For solving caffe compilation problem "No to_python (by-value) converter found" see:
# https://github.com/BVLC/caffe/issues/3494

#RUN apt-get clean && rm -rf /var/lib/apt/lists/*
