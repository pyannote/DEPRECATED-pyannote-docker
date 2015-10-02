FROM ubuntu:15.04
MAINTAINER Hervé Bredin <bredin@limsi.fr>

RUN apt-get update
RUN apt-get install -y wget git mercurial
RUN apt-get install -y autoconf automake build-essential pkg-config cmake cmake-curses-gui nasm gfortran

# ==== PYTHON =================================================================

RUN apt-get install -y python python-dev python-pip python-virtualenv

# ==== BOOST ==================================================================

RUN apt-get install -y libboost-dev libbost-python-dev

# ==== FFMPEG =================================================================
# https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu

RUN apt-get -y install libass-dev libfreetype6-dev libsdl1.2-dev \
                       libtheora-dev libtool libva-dev \
                       libvdpau-dev libvorbis-dev libxcb1-dev \
                       libxcb-shm0-dev libxcb-xfixes0-dev \
                       texi2html zlib1g-dev

# -- YASM --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz \
 && tar xzvf yasm-1.3.0.tar.gz \
 && cd yasm-1.3.0 \
 && ./configure \
 && make \
 && make install \
 && make distclean \
 && rm -rf /tmp/ffmpeg_sources

# -- libx264 --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget http://download.videolan.org/pub/x264/snapshots/last_x264.tar.bz2 \
 && tar xjvf last_x264.tar.bz2 \
 && cd x264-snapshot* \
 && ./configure --enable-static \
 && make \
 && make install \
 && make distclean \
 && rm -rf /tmp/ffmpeg_sources

# -- libx265 --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && hg clone https://bitbucket.org/multicoreware/x265 \
 && cd /tmp/ffmpeg_sources/x265/build/linux \
 && cmake -G "Unix Makefiles" -DENABLE_SHARED:bool=off ../../source \
 && make \
 && make install \
 && rm -rf /tmp/ffmpeg_sources

# -- libfdk-aac --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget -O fdk-aac.tar.gz https://github.com/mstorsjo/fdk-aac/tarball/master \
 && tar xzvf fdk-aac.tar.gz \
 && cd mstorsjo-fdk-aac* \
 && autoreconf -fiv \
 && ./configure --disable-shared \
 && make \
 && make install \
 && make distclean \
 && rm -rf /tmp/ffmpeg_sources

# -- libmp3lame --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget http://downloads.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz \
 && tar xzvf lame-3.99.5.tar.gz \
 && cd lame-3.99.5 \
 && ./configure --enable-nasm --disable-shared \
 && make \
 && make install \
 && make distclean \
 && rm -rf /tmp/ffmpeg_sources

# -- libopus --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget http://downloads.xiph.org/releases/opus/opus-1.1.tar.gz \
 && tar xzvf opus-1.1.tar.gz \
 && cd opus-1.1 \
 && ./configure --disable-shared \
 && make \
 && make install \
 && make clean \
 && rm -rf /tmp/ffmpeg_sources

# -- libvpx --

RUN mkdir /tmp/ffmpeg_sources \
 && cd /tmp/ffmpeg_sources \
 && wget http://storage.googleapis.com/downloads.webmproject.org/releases/webm/libvpx-1.4.0.tar.bz2 \
 && tar xjvf libvpx-1.4.0.tar.bz2 \
 && cd libvpx-1.4.0 \
 && ./configure --disable-examples --disable-unit-tests \
 && make \
 && make install \
 && make clean \
 && rm -rf /tmp/ffmpeg_sources

# -- ffmpeg --

RUN wget http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 \
 && tar xjvf ffmpeg-snapshot.tar.bz2 \
 && rm ffmpeg-snapshot.tar.bz2 \
 && cd ffmpeg \
 && ./configure \
    --pkg-config-flags="--static" \
    --enable-gpl \
    --enable-libass \
    --enable-libfdk-aac \
    --enable-libfreetype \
    --enable-libmp3lame \
    --enable-libopus \
    --enable-libtheora \
    --enable-libvorbis \
    --enable-libvpx \
    --enable-libx264 \
    --enable-libx265 \
    --enable-nonfree \
 && make \
 && make install \
 && make distclean \
 && hash -r \
 && rm -rf /tmp/ffmpeg_sources

# ==== scientific Python ======================================================

 RUN pip install numpy scipy matplotlib jupyter

# ==== OPENCV =================================================================

RUN apt-get install -y python-opencv

# ==== YAAFE ==================================================================

RUN apt-get install -y libargtable2-0 libargtable2-dev \
                       libsndfile1 libsndfile1-dev \
                       libmpg123-0 libmpg123-dev \
                       libfftw3-3 libfftw3-dev \
                       liblapack-dev

RUN wget https://github.com/Yaafe/Yaafe/archive/v0.65.1.tar.gz && \
    tar xzvf v0.65.1.tar.gz && \
    cd Yaafe-0.65.1 && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local \
          -DCMAKE_INSTALL_PYTHON_PACKAGES=/usr/local/lib/python2.7/dist-packages \
          -DCMAKE_INSTALL_YAAFE_EXTENSIONS=/usr/local/lib/python2.7/dist-packages \
          -DWITH_FFTW3=ON \
          -DWITH_LAPACK=ON \
          -DWITH_MATLAB_MEX=OFF \
          -DWITH_MPG123=ON \
          -DWITH_SNDFILE=ON \
          .. && \
    make && \
    make install && \
    cd ../.. && \
    rm -rf Yaafe-0.65.1 v0.65.1.tar.gz

# TODO: add HDF5 support (-DWITH_HDF5=ON)

ENV LD_LIBRARY_PATH /usr/local/lib
ENV YAAFE_PATH /usr/local/lib/python2.7/dist-packages
