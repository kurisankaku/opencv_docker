FROM ubuntu:18.04
ARG PYTHON_VERSION=3.7.4
ARG OPENCV_VERSION=4.1.0

RUN apt update \
  && apt install -y --no-install-recommends \
    make \
    cmake \
    gcc \
    g++ \
    unzip \
    wget \
    build-essential \
    zlib1g-dev \
    libffi-dev \
    libssl-dev \
  && apt clean \
  && rm -rf /var/lib/apt/lists/*
# python3 install
WORKDIR /tmp/python3
RUN wget --no-check-certificate https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz \
  && tar zxf Python-${PYTHON_VERSION}.tgz \
  && rm Python-${PYTHON_VERSION}.tgz
WORKDIR /tmp/python3/Python-${PYTHON_VERSION}
RUN ./configure && make altinstall && make clean
WORKDIR /usr/local/bin/
RUN ln -s /usr/local/bin/$(find -name python3.[0-9]) /bin/python3
RUN ln -s /usr/local/bin/$(find -name pip3.[0-9]) /bin/pip3
RUN pip3 install --upgrade pip numpy
# OpenCV3 install
RUN apt update \
  && apt install -y --no-install-recommends \
    ca-certificates \
    pkg-config \
    libgtk2.0-dev \
    libjpeg-dev libpng-dev \
    ffmpeg \
    libavcodec-dev \
    libavformat-dev \
    libavresample-dev \
    libswscale-dev \
    libv4l-dev \
    libtbb-dev \
  && apt clean \
  && rm -rf /var/lib/apt/lists/*
RUN mkdir -p /tmp/opencv
WORKDIR /tmp/opencv
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
  && unzip ${OPENCV_VERSION}.zip -d . \
  && mkdir /tmp/opencv/opencv-${OPENCV_VERSION}/build \
  && rm ${OPENCV_VERSION}.zip
WORKDIR /tmp/opencv/opencv-${OPENCV_VERSION}/build/
RUN cmake -D CMAKE_BUILD_TYPE=RELEASE \
  -D BUILD_TESTS=OFF \
  -D CMAKE_BUILD_TYPE=RELEASE \
  -D BUILD_PERF_TESTS=OFF \
  -D WITH_FFMPEG=ON \
  -D WITH_TBB=ON .. \
  && make -j "$(nproc)" \
  && make install \
  && make clean
WORKDIR /sampleproject
