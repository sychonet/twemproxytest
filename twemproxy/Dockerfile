FROM debian:latest
COPY twemproxy.yml /home/twemproxy.yml
RUN apt-get update
RUN apt-get -qq install git autoconf libtool make
RUN git clone https://github.com/twitter/twemproxy.git
RUN cd twemproxy && autoreconf -fvi && ./configure --enable-debug=full --disable-dependency-tracking
RUN cd twemproxy && make
CMD cd twemproxy && ./src/nutcracker --conf-file=/home/twemproxy.yml --mbuf-size=512 --stats-interval=30000 --verbose=6
EXPOSE 6390
EXPOSE 22222