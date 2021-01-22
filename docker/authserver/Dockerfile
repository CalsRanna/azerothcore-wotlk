FROM ubuntu:20.04

ENV TIME_ZONE=Asia/Shanghai
ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && apt install -y libmysqlclient-dev libssl-dev libace-6.4.5 libace-dev net-tools tzdata;
RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata

HEALTHCHECK --interval=5s --timeout=15s --start-period=30s --retries=3 CMD netstat -lnpt | grep :3724 || exit 1

CMD ["/azeroth-server/bin/authserver"]
