# docker build -t mylogstash .

FROM docker.elastic.co/logstash/logstash:6.4.2

EXPOSE 5044

COPY ./logstash.conf /usr/share/logstash/pipeline/logstash.conf

# docker run -it --rm docker.elastic.co/logstash/logstash:6.4.2 -e 'input { stdin { } } output { stdout { } }'
