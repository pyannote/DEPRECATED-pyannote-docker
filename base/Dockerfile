# The MIT License (MIT)
#
# Copyright (c) 2015 CNRS
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.
#
# AUTHORS
# Hervé Bredin (bredin@limsi.fr)

FROM ubuntu:14.04
MAINTAINER Hervé Bredin <bredin@limsi.fr>

# ENV LANGUAGE en_US.UTF-8
# ENV LANG en_US.UTF-8
# ENV LC_ALL en_US.UTF-8
# ENV PYTHONIOENCODING UTF-8
#
RUN apt-get update -qq \
 && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
      autoconf \
      automake \
      build-essential \
      cmake \
      cmake-curses-gui \
      curl \
      gfortran \
      git \
      libargtable2-0 \
      libargtable2-dev \
      libass-dev \
      libatlas-dev \
      libboost-all-dev \
      libboost-dev \
      libboost-python-dev \
      libfftw3-3 \
      libfftw3-dev \
      libfreetype6-dev \
      libgtk2.0-dev \
      libhdf5-dev \
      libjpeg-dev \
      liblapack-dev \
      libmpg123-0 \
      libmpg123-dev \
      libsdl1.2-dev \
      libsndfile1 \
      libsndfile1-dev \
      libtheora-dev \
      libtool \
      libva-dev \
      libvdpau-dev \
      libvorbis-dev \
      libxcb-shm0-dev \
      libxcb-xfixes0-dev \
      libxcb1-dev \
      mercurial \
      nasm \
      pkg-config \
      python \
      python-dev \
      python-numpy \
      python-opencv \
      python-pip \
      python-scipy \
      python-software-properties \
      python-virtualenv \
      software-properties-common \
      sudo \
      texi2html \
      wget \
      zip \
      zlib1g-dev \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN pip install matplotlib \
                jupyter \
                pandas \
                scikit-learn \
                scikit-image \
                dlib \
                numexpr \
                pyannote.core

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
          -DWITH_HDF5=ON \
          .. && \
    make && \
    make install && \
    cd ../.. && \
    rm -rf Yaafe-0.65.1 v0.65.1.tar.gz
ENV YAAFE_PATH /usr/local/lib/python2.7/dist-packages

RUN curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash -e && \
    git clone https://github.com/torch/distro.git ~/torch --recursive && \
    cd ~/torch && \
    ./install.sh && \
    ~/torch/install/bin/luarocks install nn && \
    ~/torch/install/bin/luarocks install dpnn && \
    ~/torch/install/bin/luarocks install image && \
    ~/torch/install/bin/luarocks install optim

ENV LD_LIBRARY_PATH /usr/local/lib:/opt/OpenBLAS/lib

# add torch to PATH
ENV PATH ~/torch/install/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents
# kernel crashes.
ENV TINI_VERSION v0.6.0
RUN curl -L https://github.com/krallin/tini/releases/download/v0.6.0/tini > tini && \
    echo "d5ed732199c36a1189320e6c4859f0169e950692f451c03e7854243b95f4234b *tini" | sha256sum -c - && \
    mv tini /usr/bin/tini && \
    chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]

# run jupyter notebook by default
EXPOSE 8888
VOLUME /notebook
WORKDIR /notebook
CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0"]
