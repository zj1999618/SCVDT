# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

# Docker image with head clang installed.

FROM hust-fuzz-base/base-image18
MAINTAINER yijun2238@gmail.com

#RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
#RUN apt-get clean
RUN apt-get update

#RUN apt-get install -y libc6-dev binutils libgcc-5-dev
RUN apt-get install -y build-essential make cmake ninja-build git binutils-gold
RUN apt-get install -y binutils-dev curl wget
RUN apt-get install -y python3-distutils

COPY checkout_build_install_llvm.sh /root/
# Keep all steps in the same script to decrease the number of intermediate
# layes in docker file.
RUN /bin/bash /root/checkout_build_install_llvm.sh
RUN rm /root/checkout_build_install_llvm.sh

# Setup the environment.
ENV CC "clang"
ENV CXX "clang++"
ENV CCC "clang++"

# FUZZING_BUILD_MODE_UNSAFE_FOR_PRODUCTION is described at
# http://libfuzzer.info#fuzzer-friendly-build-mode

ENV CFLAGS "-O1 -fno-omit-frame-pointer -gline-tables-only -DFUZZING_BUILD_MODE_UNSAFE_FOR_PRODUCTION"
ENV CXXFLAGS_EXTRA "-stdlib=libc++"
ENV CXXFLAGS "$CFLAGS $CXXFLAGS_EXTRA"
