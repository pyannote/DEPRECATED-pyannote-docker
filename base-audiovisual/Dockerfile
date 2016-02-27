# The MIT License (MIT)
#
# Copyright (c) 2016 CNRS
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

FROM pyannote/base-audio
MAINTAINER Hervé Bredin <bredin@limsi.fr>

# -- OpenCV --

 RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
  libgtk2.0-dev

 RUN mkdir /tmp/opencv_sources \
  && cd /tmp/opencv_sources \
  && wget https://github.com/Itseez/opencv/archive/3.1.0.tar.gz \
  && tar xzvf 3.1.0.tar.gz \
  && rm 3.1.0.tar.gz \
  && cd /tmp/opencv_sources \
  && wget https://github.com/Itseez/opencv_contrib/archive/3.1.0.tar.gz \
  && tar xzvf 3.1.0.tar.gz \
  && rm 3.1.0.tar.gz \
  && mkdir /tmp/opencv_sources/opencv-3.1.0/build \
  && cd /tmp/opencv_sources/opencv-3.1.0/build \
  && cmake -D CMAKE_BUILD_TYPE=RELEASE \
           -D CMAKE_INSTALL_PREFIX=/usr/local \
           -D INSTALL_C_EXAMPLES=OFF \
           -D INSTALL_PYTHON_EXAMPLES=ON \
           -D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv_sources/opencv_contrib-3.1.0/modules \
           -D BUILD_EXAMPLES=ON .. \
   && make -j2 \
   && make install \
   && ldconfig \
   && rm -rf /tmp/opencv_sources

 # # update OpenCV because of https://github.com/Itseez/opencv/pull/6009
 # RUN mkdir /tmp/opencv_sources \
 #  && cd /tmp/opencv_sources \
 #  && wget https://github.com/Itseez/opencv/archive/master.zip \
 #  && unzip master.zip \
 #  && rm master.zip \
 #  && cd /tmp/opencv_sources \
 #  && wget https://github.com/Itseez/opencv_contrib/archive/master.zip \
 #  && unzip master.zip \
 #  && rm master.zip \
 #  && mkdir /tmp/opencv_sources/opencv-master/build \
 #  && cd /tmp/opencv_sources/opencv-master/build \
 #  && cmake -D CMAKE_BUILD_TYPE=RELEASE \
 #           -D CMAKE_INSTALL_PREFIX=/usr/local \
 #           -D INSTALL_C_EXAMPLES=OFF \
 #           -D INSTALL_PYTHON_EXAMPLES=ON \
 #           -D OPENCV_EXTRA_MODULES_PATH=/tmp/opencv_sources/opencv_contrib-master/modules \
 #           -D BUILD_EXAMPLES=ON .. \
 #   && make -j2 \
 #   && make install \
 #   && ldconfig \
 #   && rm -rf /tmp/opencv_sources

# -- dlib --

RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    libboost-dev \
    libboost-python-dev

RUN pip install dlib
