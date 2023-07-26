FROM elasticsearch:7.17.10

RUN mkdir -p /etc/elasticsearch/config
COPY ./config/ /usr/share/elasticsearch/config/

RUN set -ex \
    && for path in \
        /usr/share/elasticsearch/config/ \
    ; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done
