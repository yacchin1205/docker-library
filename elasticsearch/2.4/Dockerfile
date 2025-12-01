# Updated: 2024-12 - removed apt-get (stretch repos EOL)
FROM elasticsearch:2.4.5

RUN bin/plugin install io.fabric8/elasticsearch-cloud-kubernetes/2.4.5_01
RUN bin/plugin install lmenezes/elasticsearch-kopf/2.1.2

ENV BOOTSTRAP_MLOCKALL=false
ENV JAVA_OPTS=-Djava.net.preferIPv4Stack=true

ADD elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml
