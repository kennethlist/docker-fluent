docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-v /path/elasticsearch:/usr/share/elasticsearch/data \
-d elasticsearch

docker run -d --name fluentd -p 24224:24224 \
--link elasticsearch -v /var/lib/docker/containers:/var/lib/docker/containers docker-fluentd

docker run -d -p 5601:5601 \
    -e ELASTICSEARCH_URL='http://elasticsearch:9200' \
    --name kibana \
    --link elasticsearch \
    kibana

docker run                                      \
  --volume=/:/rootfs:ro                         \
  --volume=/var/run:/var/run:rw                 \
  --volume=/sys:/sys:ro                         \
  --volume=/var/lib/docker/:/var/lib/docker:ro  \
  --publish=9081:8080                           \
  --detach=true                                 \
  --name=cadvisor                               \
  google/cadvisor:latest