############################################################
# Dockerfile to run ESTNeT simulations 
# including Omnet++ 5.5.1, INET 4.2.0 and ESTNeT Beta 0.9
# Based on Ubuntu 19.10
############################################################
FROM docker.pkg.github.com/estnet-framework/estnet-docker/ubuntu_opp_inet_base:v0.9 as base
LABEL maintainer="Timon Petermann <timonpetermann@aol.com>"

# first stage - build estnet
FROM base as builder
ARG VERSION=0.9
WORKDIR /workspace
RUN apt-get -q update && apt-get -qy upgrade && apt-get -qy install wget && apt-get -qy install git
RUN wget https://github.com/estnet-framework/estnet/archive/v$VERSION.tar.gz \
         -O estnet-src.tgz --progress=dot:mega && \
         tar xf estnet-src.tgz && rm estnet-src.tgz && \
         mv estnet-0.9/ estnet
WORKDIR /workspace/estnet
ARG INET_PROJ=/workspace/inet4
RUN opp_makemake --nolink -f --deep -O out -KINET_PROJ=$INET_PROJ -DINET_IMPORT \
         -DESTNET_IMPORT -I. -Isrc/estnet/common -I$INET_PROJ/src -L$INET_PROJ/src \
         -Lout/$CONFIGNAME/src -lINET$D -d src -X. && \
         cd src && \
         opp_makemake --make-so -f --deep -o ESTNeT -O out -pESTNET -KINET_PROJ=$INET_PROJ -DINET_IMPORT -I. -Iestnet/common -I$INET_PROJ/src -L$INET_PROJ/src -lINET$D && \
         cd ..

RUN make -j$(nproc) MODE=release && \
    make -j$(nproc) MODE=debug
RUN sed -i 's/osgi\.instance\.area\.default=\.\.\/samples/osgi\.instance\.area\.default=\/workspace/' /omnetpp-5.5.1/ide/configuration/config.ini && \
    sed -i 's/osgi\.instance\.area\.default=$IDEDIR\/\.\.\/samples/osgi\.instance\.area\.default=\/workspace/' /omnetpp-5.5.1/bin/omnetpp
WORKDIR /workspace
RUN git clone https://github.com/estnet-framework/estnet-template.git



# second stage - copy the final binaries 
FROM base
RUN mkdir -p /workspace/estnet
RUN mkdir -p /workspace/estnet-template
WORKDIR /workspace/estnet
COPY --from=builder /workspace/estnet/ .
WORKDIR /workspace/estnet-template
COPY --from=builder /workspace/estnet-template/ .
COPY --from=builder /omnetpp-5.5.1/ide/configuration/config.ini /omnetpp-5.5.1/ide/configuration/config.ini
COPY --from=builder /omnetpp-5.5.1/bin/omnetpp /omnetpp-5.5.1/bin/omnetpp
RUN sed -i 's/<project>inet<\/project>/<project>inet4<\/project>/' /workspace/estnet/.project
WORKDIR /workspace
ENV ESTNET_VER=0.9
