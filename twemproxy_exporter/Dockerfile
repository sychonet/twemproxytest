FROM debian:latest
RUN apt-get update
RUN apt-get -qq install git libtool wget
RUN wget https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz
RUN rm go1.19.3.linux-amd64.tar.gz
RUN git clone https://github.com/stuartnelson3/twemproxy_exporter
RUN cd twemproxy_exporter && /usr/local/go/bin/go build -o twemproxy_exporter 
CMD cd twemproxy_exporter && ./twemproxy_exporter -twemproxy.timeout 5s -twemproxy.stats-address "twemproxy:22222" -web.listen-address ":9151" -web.telemetry-path "/metrics"