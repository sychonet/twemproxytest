# twemproxytest
Project to analyse twemproxy performance on local containerized setup. 


`cd twemproxy/src && ./nutcracker --conf-file=/home/twemproxy.yml --stats-interval=5000 --mbuf-size=16384 --daemonize`

`docker run --rm --network=twemproxytest redislabs/memtier_benchmark:latest --server=twemproxy --port=6390 --protocol=redis --run-count=2 --show-config --requests=5000 --clients=50 --threads=4 --test-time=60 --ratio=1:1 --reconnect-interval=1000 --data-size-range=100-1024 --random-data --expiry-range=5-10 --key-prefix=memtier-benchmark- --key-minimum=0 --key-maximum=100000000 --print-percentiles=95,99,99.9` 