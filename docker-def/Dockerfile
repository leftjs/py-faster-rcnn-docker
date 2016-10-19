FROM nvidia/cuda:7.5-cudnn3-devel

COPY files/.screenrc /root/.screenrc

RUN apt-get update && apt-get install -y screen wget nano libboost-all-dev libgoogle-glog-dev liblmdb-dev git openssh-client libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler bundler

RUN wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86_64.sh -O ~/anaconda2.sh
RUN bash ~/anaconda2.sh -b -p ~/anaconda2
ENV PATH /root/anaconda2/bin:${PATH}
RUN conda install -y openblas protobuf python-gflags hdf5 opencv snappy boost numpy
COPY files/caffe-requirements.txt /tmp/caffe-requirements.txt
RUN pip install -r /tmp/caffe-requirements.txt
RUN pip install easydict
RUN rm ~/anaconda2.sh /tmp/caffe-requirements.txt

# Installing lippng16
RUN cd /root
RUN git clone https://github.com/dimensio/libpng16-deb.git
RUN cd libpng16-deb
RUN bundle install
RUN make

# For solving caffe compilation problem with float128 see:
# http://www.pcl-users.org/Installation-Trouble-Boost-float128-td4040519.html

# For solving caffe compilation problem with libpng16-deb see:
# http://stackoverflow.com/questions/31962975/caffe-install-on-ubuntu-for-anaconda-with-python-2-7-fails-with-libpng16-so-16-n

# For solving caffe compilation problem "No to_python (by-value) converter found" see:
# https://github.com/BVLC/caffe/issues/3494

#RUN apt-get clean && rm -rf /var/lib/apt/lists/*
