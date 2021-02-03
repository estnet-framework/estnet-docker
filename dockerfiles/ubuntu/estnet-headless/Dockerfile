############################################################
# Dockerfile to run ESTNeT simulations 
# including Omnet++ 5.6.2 and INET 4.2.0
# additionally, ESTNeT is already compiled in Release and 
# Debug
############################################################
FROM omnetpp/inet-gui:o5.6.2-4.2.0 as base
LABEL maintainer="Timon Petermann <timonpetermann@aol.com>"

# first stage - build estnet
FROM base as builder
ARG VERSION=0.9
WORKDIR /root
RUN wget https://github.com/estnet-framework/estnet/archive/v$VERSION.tar.gz \
         -O estnet-src.tgz --progress=dot:mega && \
         tar xf estnet-src.tgz && rm estnet-src.tgz && \
         mv estnet-0.9/ estnet
WORKDIR /root/estnet
ARG INET_PROJ=/root/inet4
RUN opp_makemake --nolink -f --deep -O out -KINET_PROJ=$INET_PROJ -DINET_IMPORT \
         -DESTNET_IMPORT -I. -Isrc/estnet/common -I$INET_PROJ/src -L$INET_PROJ/src \
         -Lout/$CONFIGNAME/src -lINET$D -d src -X. && \
         cd src && \
         opp_makemake --make-so -f --deep -o ESTNeT -O out -pESTNET -KINET_PROJ=$INET_PROJ -DINET_IMPORT -I. -Iestnet/common -I$INET_PROJ/src -L$INET_PROJ/src -lINET$D && \
         cd ..

RUN make -j$(nproc) MODE=release && \
    make -j$(nproc) MODE=debug && \
    rm -rf out


# second stage - copy only the final binaries (to get rid of the 'out' folder and reduce the image size)
FROM base
RUN mkdir -p /root/estnet
WORKDIR /root/estnet
COPY --from=builder /root/estnet/ .
ENV ESTNET_VER=0.9

