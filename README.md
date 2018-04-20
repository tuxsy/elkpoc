# elkpoc


docker run -d --name elk-poc -p 9200:9200 java:8 tail -F /var/log/syslog

## Instalar Elasticsearch

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -

apt-get update
apt-get install -y vim apt-transport-https

echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list

apt-get update
apt-get install -y elasticsearch


vim /etc/elasticsearch/elasticsearch.yml
-- Añadir
  network.host: "localhost"
  http.port:9200


service elasticsearch start






REQUISITOS
sudo sysctl -w vm.max_map_count=262144


docker run -d --name elk-poc-elasticsearch -e ELASTIC_PASSWORD=1234 -p 9200:9200 docker.elastic.co/elasticsearch/elasticsearch-platinum:6.2.4

docker run -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro --link elk-poc-elasticsearch:elasticsearch -h kibana --name elk-poc-logstash_and_kibana -p 5601:5601 java:8 /sbin/init

docker exec -ti elk-poc-logstash_and_kibana  bash
  apt-get update && apt-get install -y wget vim apt-transport-https

  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
  echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list

  apt-get update && apt-get install -y logstash kibana metricbeat

  vim /etc/kibana/kibana.yml
    server.port: 5601
    server.host: "kibana"
    elasticsearch.url: "http://elasticsearch:9200"
    elasticsearch.username: "elastic"
    elasticsearch.password: "1234"

  service kibana start

  vim /etc/metricbeat/metricbeat.yml
    output.elasticsearch:
      hosts: ["elasticsearch:9200"]
      username: "elastic"
      password: "1234

  service metricbeat start


----------- CONFIGURAR LOGSTASH PARA COPIAR LOS LOGS DE APACHE
systemctl start logstash