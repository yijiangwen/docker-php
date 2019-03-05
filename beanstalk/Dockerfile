FROM alpine

LABEL maintainer="ninja911<ninja911@qq.com>"

RUN apk add --no-cache tini beanstalkd xdg-utils curl wget


RUN mkdir -p /usr/local/aurora/ && \
    cd /usr/local/aurora/ && \
    wget --no-check-certificate http://www.ninja911.com/downloads/aurora_linux_amd64_v2.2.tar.gz && \
    tar -zxf /usr/local/aurora/aurora_linux_amd64_v2.2.tar.gz && \
    chmod +x /usr/local/aurora/aurora && \
    rm -f /usr/local/aurora/aurora_linux_amd64_v2.2.tar.gz

# 启动aurora命令
# /usr/local/aurora/aurora -c /usr/local/aurora/conf/aurora.toml

WORKDIR /usr/local/aurora

EXPOSE ${BEANSTALKD_PORT} 3000

ENTRYPOINT ["/sbin/tini", "--"]

CMD /usr/bin/beanstalkd -l $BEANSTALKD_ADDR -p $BEANSTALKD_PORT -z $BEANSTALKD_MAX_JOB_SIZE