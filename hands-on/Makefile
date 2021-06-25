build-elasticsearch:
		docker build -t opstree/elasticsearch:server -f Dockerfile.elastic .

run-elasticsearch:
		docker rm -f elasticsearch || true
		docker run -itd --name elasticsearch -p 9200:9200 --network=uat-net opstree/elasticsearch:server

build-logstash:
		docker build -t opstree/logstash:server -f Dockerfile.logstash .

run-logstash:
		docker rm -f logstash || true
		docker run -itd --name logstash --link elasticsearch:elasticsearch -p 5044:5044 --network=uat-net opstree/logstash:server

build-nginx:
		docker build -t opstree/nginx:server -f Dockerfile.nginx .

run-nginx:
		docker rm -f nginx || true
		docker run -itd --name nginx --link logstash:logstash -p 80:80 --network=uat-net --log-driver=syslog --log-opt syslog-address=udp://logstash:5044 opstree/nginx:server

build-kibana:
		docker build -t opstree/kibana:server -f Dockerfile.kibana .

run-kibana:
		docker rm -f kibana || true
		docker run -itd --name kibana --link elasticsearch:elasticsearch -p 5601:5601 --network=uat-net opstree/kibana:server

run-all:
		make build-elasticsearch
		make build-logstash
		make build-nginx
		make build-kibana
		make run-elasticsearch
		make run-logstash
		make run-nginx
		make run-kibana
