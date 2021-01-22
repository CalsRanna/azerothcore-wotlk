FROM alpine:3.9 as builder

RUN apk add --no-cache bash

COPY apps /azerothcore/apps
COPY bin /azerothcore/bin
COPY conf /azerothcore/conf
COPY data /azerothcore/data
COPY deps /azerothcore/deps
COPY acore.json /azerothcore/acore.json

RUN ./azerothcore/bin/acore-db-asm 1

FROM mysql:5.7

ENV TZ=Asia/Shanghai

ENV LANG C.UTF-8

COPY --from=builder /azerothcore/env/dist/sql /sql
COPY docker/database/generate-databases.sh /docker-entrypoint-initdb.d

HEALTHCHECK --interval=5s --timeout=15s --start-period=30s --retries=3 CMD mysqladmin -uroot -p$MYSQL_ROOT_PASSWORD ping -h localhost
