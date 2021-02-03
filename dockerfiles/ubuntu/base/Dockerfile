############################################################
# Dockerfile to run ESTNeT simulations 
# including Omnet++ 5.5.1 and INET 4.2.0
# The local ESTNeT path can be mounted by using the script
# in the /util directory
# Based on Ubuntu 19.10
############################################################
FROM ubuntu:19.10
LABEL maintainer="Timon Petermann <timonpetermann@aol.com>"
RUN sed -i -e 's/archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install g++-7 libc6-dev make dpkg-dev curl bison flex qt5-default qt5-qmake zlib1g-dev mpi-default-dev
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install python3.8 python3-pip
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install openjdk-8-jre
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install openscenegraph-3.4 osgearth openscenegraph-plugin-osgearth libopenscenegraph-3.4-dev libosgearth-dev libxml2-dev
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install sudo
RUN groupadd -r wheel && echo '%wheel ALL=(ALL) ALL' >> /etc/sudoers
ENV PATH=/omnetpp-5.5.1/bin:$PATH \
    LD_LIBRARY_PATH=/omnetpp-5.5.1/lib
RUN curl -L https://github.com/omnetpp/omnetpp/releases/download/omnetpp-5.5.1/omnetpp-5.5.1-src-linux.tgz | tar -C / -xz
RUN cd /omnetpp-5.5.1 && CC=gcc-7 CXX='g++-7' ./configure PREFER_CLANG=no PREFER_SQLITE_RESULT_FILES=yes
RUN make -C /omnetpp-5.5.1
RUN ln -s /usr/bin/python3.8 /usr/bin/python
RUN mkdir /workspace && \
  curl -L https://github.com/inet-framework/inet/releases/download/v4.2.0/inet-4.2.0-src.tgz | tar -C /workspace -xz
RUN make -C /workspace/inet4 makefiles
RUN make -C /workspace/inet4 MODE=debug
RUN make -C /workspace/inet4 MODE=release
RUN echo 'export PATH=/omnetpp-5.5.1/bin:$PATH\nexport LD_LIBRARY_PATH=/omnetpp-5.5.1/lib' >> /etc/profile
RUN python -m pip install pytest numpy scipy matplotlib
ENV MESA_GL_VERSION_OVERRIDE=3.3 \
  MESA_GLSL_VERSION_OVERRIDE=330
