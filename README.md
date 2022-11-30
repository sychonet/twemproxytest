# twemproxytest

[**twemproxy**](https://github.com/twitter/twemproxy) (pronounced "two-em-proxy"), aka **nutcracker** is a fast and lightweight proxy for [redis](http://redis.io/) protocol. It was built primarily to reduce the number of connections to the caching servers on the backend. This, together with protocol pipelining and sharding enables you to horizontally scale your distributed caching architecture. This project can be used to analyse twemproxy performance and functionality in a local setup on Docker. Following are the services used to accomplish that:
1. 3 containers of [Redis](https://hub.docker.com/_/redis)
2. 1 container of [twemproxy](https://github.com/twitter/twemproxy)
3. 1 container of [twemproxy_exporter](https://github.com/stuartnelson3/twemproxy_exporter) This service is used for exporting metrics from twemproxy to prometheus time series database.
4. 1 container of [Prometheus](https://prometheus.io/)
5. 1 container of [Grafana](https://grafana.com/) 
6. 1 container of [memtier_benchmark](https://github.com/RedisLabs/memtier_benchmark) This service is used for load testing and benchmarking of twemproxy.
7. 1 container of [redis_exporter](https://github.com/oliver006/redis_exporter) for exporting redis metrics to prometheus.

## Work in progress

1. Publish dashboard for monitoring twemproxy metrics on Grafana.
2. Add support for monitoring container metrics on Grafana.
3. Automate setup for Grafana as right now it requires a lot of manual steps to add prometheus as data source and add dashboard in it. If it can't be automated then steps will be added in this document on how to create the dashboards manually.
4. Even though the redis exporter is exporting metrics to prometheus still an error message is appearing in logs as `time="2022-11-30T05:38:48Z" level=error msg="Couldn't connect to redis instance (redis://localhost:6379)"`. This need to be checked and fixed.

## Steps to run the project

1. Install git, docker engine and docker-compose on your machine
2. Clone repository on your local machine.
3. Run command `docker-compose up` from the root directory of project.
4. Once all the containers are up then check on prometheus console at `http://localhost:9090` if the targets are working fine.
5. In case you want to use Grafana for monitoring Redis then first add Prometheus as data source and use dashboard in [link](https://grafana.com/grafana/dashboards/11835-redis-dashboard-for-prometheus-redis-exporter-helm-stable-redis-ha/)
6. Do benchmarking and run load tests on your setup using following command. Please change the values for command line arguments based on your requirements.
`docker run --rm --network=twemproxytest redislabs/memtier_benchmark:latest --server=twemproxy --port=6390 --protocol=redis --run-count=2 --show-config --requests=5000 --clients=50 --threads=4 --test-time=60 --ratio=1:1 --reconnect-interval=1000 --data-size-range=100-1024 --random-data --expiry-range=5-10 --key-prefix=memtier-benchmark- --key-minimum=0 --key-maximum=100000000 --print-percentiles=95,99,99.9` 