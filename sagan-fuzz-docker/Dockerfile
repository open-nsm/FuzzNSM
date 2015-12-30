# bro
#
# VERSION               0.1

FROM debian:sid
MAINTAINER Justin Azoff <justin.azoff@gmail.com>

ENV WD /scratch

RUN mkdir ${WD}
WORKDIR /scratch

RUN apt-get update && apt-get -y upgrade && echo 2015-11-01
RUN apt-get -y install build-essential clang
RUN apt-get -y install afl gnuplot- gnuplot5- --no-install-suggests
RUN apt-get -y install git
RUN apt-get -y install cmake
RUN apt-get -y install bison flex libssl-dev libpcap-dev

RUN apt-get -y install libpcre3-dev liblognorm-dev
RUN apt-get -y install pkg-config

RUN apt-get -y install wget
RUN wget https://quadrantsec.com/download/sagan-current.tar.gz
RUN tar xvzf sagan-current.tar.gz
RUN ln -s sagan-*/ sagan

RUN wget http://libestr.adiscon.com/files/download/libestr-0.1.10.tar.gz
RUN tar xzf libestr-0.1.10.tar.gz
RUN cd libes*/ && ./configure && make install
RUN ldconfig

RUN echo 'deb-src http://httpredir.debian.org/debian sid main' >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y build-dep afl
RUN apt-get -y -b source afl
RUN dpkg -i afl*.deb

RUN cd sagan && CC=afl-clang-fast ./configure --disable-libdnet && make install || true

RUN cd /usr/local/etc/ && wget https://quadrantsec.com/rules/sagan-rules-current.tar.gz
RUN cd /usr/local/etc/ && tar xzf sagan-rules-current.tar.gz && mv rules sagan-rules

RUN cat /usr/local/etc/sagan.conf | grep -v bluedot > a && mv a /usr/local/etc/sagan.conf

ADD sagan_afl.patch /scratch/sagan_afl.patch
RUN patch -p0 < sagan_afl.patch
RUN cd sagan && make install

RUN mkdir /scratch/input /scratch/output
ADD some_log /scratch/input/i

CMD /bin/bash -l
