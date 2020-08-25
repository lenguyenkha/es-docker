FROM docker.elastic.co/elasticsearch/elasticsearch:7.7.0

RUN bin/elasticsearch-plugin install analysis-icu
RUN bin/elasticsearch-plugin install analysis-kuromoji

COPY analysis-sudachi-elasticsearch7.7-2.0.2.1.zip /usr/share/elasticsearch/analysis-sudachi-elasticsearch7.7-2.0.2.1.zip
RUN bin/elasticsearch-plugin install file:/usr/share/elasticsearch/analysis-sudachi-elasticsearch7.7-2.0.2.1.zip

EXPOSE 9200 9300

