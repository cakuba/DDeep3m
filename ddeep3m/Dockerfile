FROM cuda9-cudnn7:latest
LABEL maintainer "<cakuba@qq.com>"

# updated source list
ADD sources.list /etc/apt/

# pre-requirements
RUN chmod 777 /tmp && \
    rm -fr /var/lib/apt/lists/* && \
    apt-get clean && apt-get update --fix-missing && apt-get install -y --no-install-recommends \
    git vim python-pip unzip parallel python-opencv time \
    graphicsmagick libgraphicsmagick1-dev libgraphicsmagick++1-dev openjdk-8-jdk \
    gcc g++ gfortran libbz2-dev libhdf5-dev libfftw3-dev libreadline6-dev \
    libgflags-dev libgoogle-glog-dev liblmdb-dev libopencv-dev texinfo \
    libprotobuf-dev protobuf-compiler libtiff-dev gperf flex bison gnuplot \
    libjpeg62 libfreetype6-dev libncurses5-dev libfltk1.3-dev libleveldb-dev \
    libhdf5-serial-dev libboost-all-dev libatlas-base-dev libsnappy-dev && \
    pip install joblib requests 
    
# octave 4.0.3
RUN cd /usr/local/src && \
    wget ftp://ftp.gnu.org/gnu/octave/octave-4.0.3.tar.gz && \
    tar -zxvf octave-4.0.3.tar.gz && \
    cd octave-4.0.3 && \
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 && \
    ./configure --prefix=/usr --with-hdf5-includedir=/usr/include/hdf5/serial \ 
                --with-hdf5-libdir=/usr/lib/x86_64-linux-gnu/hdf5/serial && \
    while [ -e confdir3/confdir3 ]; do mv confdir3/confdir3 confdir3a; \
          rmdir confdir3; mv confdir3a confdir3; done; rmdir confdir3 && \
    make -j4 && \
    make install && \
    wget https://excellmedia.dl.sourceforge.net/project/octave/Octave%20Forge%20Packages/Individual%20Package%20Releases/image-2.4.1.tar.gz &&\
    octave --silent --no-gui --texi-macros-file=/dev/null --eval "pkg install image-2.4.1.tar.gz" &&\
    cd /usr/local/src && rm -fr octave-4.0.3*

# hdf5oct
RUN cd /usr/local/src && \
    wget https://github.com/stegro/hdf5oct/archive/b047e6e611e874b02740e7465f5d139e74f9765f.zip && \
    unzip b047e6e611e874b02740e7465f5d139e74f9765f.zip  && \
    cd hdf5oct-b047e6e611e874b02740e7465f5d139e74f9765f && \
    make && make install && \
    cd /usr/local/src && rm -fr b047e6e611e874b02740e7465f5d139e74f9765f.zip hdf5oct-b047e6e611e874b02740e7465f5d139e74f9765f

# bats
RUN cd /usr/local/src && \
    wget https://github.com/bats-core/bats-core/archive/v0.4.0.tar.gz && \
    tar -zxvf v0.4.0.tar.gz  && \
    cd bats-core-0.4.0 && \
    ./install.sh /usr/local && \
    cd /usr/local/src && rm -fr v0.4.0.tar.gz bats-core-0.4.0

# special version of Caffe
RUN cd /usr/local/src && \
    git clone https://github.com/coleslaw481/caffe_nd_sense_segmentation.git && \
    cd caffe_nd_sense_segmentation && \
    cp Makefile.config.example Makefile.config && \
    sed -i 's/\/usr\/local\/include/\/usr\/local\/include \/usr\/include\/hdf5\/serial/g' Makefile.config && \
    sed -i 's/\/usr\/lib/\/usr\/lib \/usr\/lib\/x86_64-linux-gnu \/usr\/lib\/x86_64-linux-gnu\/hdf5\/serial/g' Makefile.config && \
    sed -i 's/hdf5_hl hdf5/hdf5_serial_hl hdf5_serial/g' Makefile && \
    make -j4 all && make -j4 distribute && \
    mv distribute /usr/local/src/CAFFE && \
    cd /usr/local/src/ && rm -fr caffe_nd_sense_segmentation

# installation of CDeep3M
RUN cd /usr/local/src && \
    wget https://github.com/CRBS/cdeep3m/archive/v1.6.2.tar.gz && \
    tar -zxf v1.6.2.tar.gz && \
    cd cdeep3m-1.6.2 && \
    sed -i 's/--solver=\$model_dir\/solver.prototxt/--solver=\$script_dir\/\$model_dir\/solver.prototxt/g' caffetrain.sh && \
    sed -i 's/\${model_dir}\/log\/out.log/\$script_dir\/\$log_dir\/out.log/g' caffetrain.sh && \
    echo "export PATH=$PATH:`pwd`:/usr/local/src/CAFFE/bin" >> /root/.bashrc && \
    echo "export CAFFE_PATH=/usr/local/src/CAFFE" >> /root/.bashrc
