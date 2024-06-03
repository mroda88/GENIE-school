FROM ubuntu:22.04
RUN rm /bin/sh && ln -s /bin/bash /bin/sh &&\
    # DEPENDENCIES
    apt-get -y update &&\
    apt-get -y upgrade &&\
    apt-get -y install git &&\
    apt-get -y install dpkg-dev &&\
    apt-get -y install cmake &&\
    apt-get -y install g++ &&\
    apt-get -y install gcc &&\
    apt-get -y install binutils &&\
    apt-get -y install libx11-dev &&\
    apt-get -y install libxpm-dev &&\
    apt-get -y install libxft-dev &&\
    apt-get -y install libxext-dev &&\
    apt-get -y install python3 &&\
    apt-get -y install libssl-dev &&\
    apt-get -y install gfortran  &&\
    apt-get -y install libpcre3-dev  &&\
    apt-get -y install xlibmesa-glu-dev  &&\
    apt-get -y install libglew-dev &&\
    apt-get -y install libftgl-dev  &&\
    apt-get -y install libmysqlclient-dev  &&\
    apt-get -y install libfftw3-dev  &&\
    apt-get -y install libcfitsio-dev  &&\
    apt-get -y install graphviz-dev  &&\
    apt-get -y install libavahi-compat-libdnssd-dev  &&\
    apt-get -y install libldap2-dev  &&\
    apt-get -y install python3-dev &&\
    apt-get -y install libxml2-dev  &&\
    apt-get -y install libxml2 &&\
    apt-get -y install libkrb5-dev  &&\
    apt-get -y install libgsl0-dev &&\
    apt-get -y install python2-dev &&\
    apt-get -y install python2 &&\
    apt-get -y install python-dev-is-python3 &&\
    apt-get -y install wget &&\
    apt-get -y install libtbb-dev &&\
    apt-get -y install libglu1-mesa-dev &&\
    apt-get -y install python3-numpy &&\
    apt-get -y install libgsl-dev &&\
    apt-get -y install libgsl-dev  &&\
    apt-get -y install nlohmann-json3-dev  &&\
    apt-get -y install libmysqlclient-dev &&\
    apt-get -y install build-essential &&\
    apt-get -y install liblog4cpp5-dev
    
    # LHAPDF
RUN mkdir /INSS &&\
    cd /INSS &&\
    wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.5.4.tar.gz -O LHAPDF-6.5.4.tar.gz &&\
    tar xf LHAPDF-6.5.4.tar.gz &&\
    cd /INSS/LHAPDF-6.5.4 &&\
    mkdir install &&\
    ./configure --prefix=/INSS/LHAPDF-6.5.4/install &&\
    make &&\
    make install &&\
    cd /INSS &&\
    # LOG4CPP
    git clone https://github.com/orocos-toolchain/log4cpp.git &&\
    cd /INSS/log4cpp &&\
    mkdir build &&\
    cd /INSS/log4cpp/build &&\
    cmake .. &&\
    make &&\
    make install &&\
    cd /INSS/ &&\
    # GENIE SOURCE
    git clone https://github.com/GENIE-MC/Generator.git --branch R-3_04_02 &&\
    cd /INSS &&\
    # PYTHIA6
    source /INSS/Generator/src/scripts/build/ext/build_pythia6.sh &&\
    cd /INSS &&\
    # ROOT
    git clone --branch v6-30-00-patches --depth=1 https://github.com/root-project/root.git ./root &&\
    mkdir /INSS/root/install &&\
    cd /INSS/root/build &&\
    cmake ../ -DPYTHIA6_DIR=/INSS/v6_428 -Dpythia6=ON -Dmathmore=ON -DCMAKE_INSTALL_PREFIX=../install &&\
    make -j4 &&\
    make install &&\
    source /INSS/root/install/bin/thisroot.sh &&\
    cd /INSS &&\
    # GENIE
    export GENIE=/INSS/Generator &&\
    export LHAPDF=/INSS/LHAPDF-6.5.4 &&\
    export PATH=$LHAPDF/bin:$PATH &&\
    export LD_LIBRARY_PATH=$LHAPDF/install/lib:$LD_LIBRARY_PATH &&\
    export LHAPDF_DATA_PATH=$LHAPDF/share/LHAPDF &&\
    export LD_LIBRARY_PATH=/INSS/LHAPDF-6.5.4/installation/lib:$LD_LIBRARY_PATH &&\
    export LD_LIBRARY_PATH=/INSS/v6_428/lib:$LD_LIBRARY_PATH &&\
    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH &&\
    export GENIE_INSTALL_DIR=/INSS/Generator/installation/ &&\
    export LD_LIBRARY_PATH=$GENIE_INSTALL_DIR/lib:$LD_LIBRARY_PATH &&\
    cd /INSS/Generator &&\
    ./configure \
    --prefix=/INSS/Generator/installation/ \
    --disable-profiler \
    --disable-validation-tools \
    --disable-cernlib \
    --disable-lhapdf5 \
    --enable-lhapdf6 \
    --enable-flux-drivers \
    --enable-geom-drivers \
    --disable-doxygen \
    --enable-test \
    --enable-mueloss \
    --enable-dylibversion \
    --enable-t2k \
    --enable-fnal \
    --enable-atmo \
    --enable-nucleon-decay \
    --disable-masterclass \
    --disable-debug \
    --with-optimiz-level=O2 \
    --with-pythia6-lib=/INSS/v6_428/lib/ \
    --with-lhapdf6-inc=/INSS/LHAPDF-6.5.4/include/ \
    --with-lhapdf6-lib=/INSS/LHAPDF-6.5.4/installation/lib/ \
    --with-libxml2-inc=/usr/include/libxml2/ \
    --with-libxml2-lib=/usr/lib/ \
    --with-log4cpp-inc=/usr/local/include/log4cpp/ \
    --with-log4cpp-lib=/usr/local/lib/ &&\
    make -j6 &&\
    make install &&\
    cd /INSS

RUN echo "source /INSS/root/install/bin/thisroot.sh" >> ~/.bashrc
WORKDIR /INSS