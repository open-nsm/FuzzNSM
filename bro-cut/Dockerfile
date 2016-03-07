# bro
#
# VERSION               0.1

FROM debian:sid
MAINTAINER Justin Azoff <justin.azoff@gmail.com>


RUN apt-get update && apt-get -y upgrade && echo 2016-03-06
RUN apt-get -y install build-essential clang
RUN apt-get -y install afl gnuplot- gnuplot5- --no-install-suggests
RUN apt-get -y install git

#I forget why I do this, something about the standard one not being built with clang support?
RUN echo 'deb-src http://httpredir.debian.org/debian sid main' >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y build-dep afl
RUN apt-get -y -b source afl
RUN dpkg -i afl*.deb

RUN apt-get -y install wget

ENV WD /scratch
WORKDIR ${WD}
RUN mkdir -p ${WD}

RUN wget -O bro-cut-2.4.0.c https://raw.githubusercontent.com/bro/bro-aux/4ffd37b4155645b47879170098886bec6bc27086/bro-cut/bro-cut.c
RUN wget -O bro-cut-2.4.1.c https://raw.githubusercontent.com/bro/bro-aux/055b73c6932b917165bc33e659fb1e4f7dc2a486/bro-cut/bro-cut.c

RUN afl-clang-fast -o bro-cut-2.4.0 bro-cut-2.4.0.c
RUN afl-clang-fast -o bro-cut-2.4.1 bro-cut-2.4.1.c

RUN mkdir ${WD}/input /${WD}/output
ADD small.log ${WD}/input/i.log

CMD /bin/bash -l
