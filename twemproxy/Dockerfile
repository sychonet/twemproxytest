FROM debian:latest
COPY twemproxy.yml home/twemproxy.yml
RUN apt-get update
RUN apt-get -qq install git autoconf libtool make
RUN git clone https://github.com/twitter/twemproxy.git
RUN cd twemproxy && autoreconf -fvi && ./configure --enable-debug=full --disable-dependency-tracking
RUN cd twemproxy && make
CMD cd twemproxy && ./src/nutcracker --mbuf-size=16384 --stats-interval=30000
EXPOSE 6390
EXPOSE 22222