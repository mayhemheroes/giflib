# Copyright 2018 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder
RUN apt-get update -y && apt-get install -y make autoconf automake libtool wget zlib1g-dev binutils cmake ninja-build liblzma-dev libz-dev pkg-config
COPY . $SRC/giflib-code
RUN rm -rf $SRC/giflib-code/mayhem
RUN git clone --depth 1 https://github.com/google/libprotobuf-mutator.git
RUN (mkdir LPM && cd LPM && cmake ../libprotobuf-mutator -GNinja -DLIB_PROTO_MUTATOR_DOWNLOAD_PROTOBUF=ON -DLIB_PROTO_MUTATOR_TESTING=OFF -DCMAKE_BUILD_TYPE=Release && ninja)
COPY mayhem/*.c mayhem/*.options mayhem/build.sh mayhem/*.h mayhem/*.cc mayhem/*.cpp mayhem/*.proto $SRC/

ENV FUZZING_LANGUAGE=c++
RUN compile
RUN chmod +x /out/dgif_protobuf_target
