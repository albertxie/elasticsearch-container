FROM docker.elastic.co/logstash/logstash:7.6.2

COPY ./config/logstash.yml /usr/share/logstash/config/logstash.yml
COPY ./pipeline/* /usr/share/logstash/pipeline

COPY ./data/* .

CMD ["./bin/logstash", "-f", "/usr/share/logstash/netflix_title_importer.conf"]
