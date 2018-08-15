FROM nginx:latest

LABEL maintainer="ninja911<ninja911@qq.com>"

ENV TIMEZONE Asia/Shanghai

# replace sources.list
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone

# init
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    openssl \
    vim \
    expect \
    && rm -rf /var/lib/apt/lists/*

# 替换VIM鼠标状态为非可视状态
RUN sed -i 's/mouse\=a/mouse=v/g' /usr/share/vim/vim80/defaults.vim

COPY docker-make-ssl.sh /usr/local/bin/

#RUN mkdir -p /etc/nginx/ssl/certs && mkdir -p /etc/nginx/ssl/crl && mkdir -p /etc/nginx/ssl/private

# 替换opensll 证书生成目录
RUN sed -i 's/\.\/demoCA/\/etc\/nginx\/ssl/g' /etc/ssl/openssl.cnf


#COPY docker-entrypoint.sh /usr/local/bin/
#RUN ln -s /usr/local/bin/docker-entrypoint.sh /entrypoint.sh # backwards compat
#ENTRYPOINT ["docker-entrypoint.sh"]


EXPOSE 80
EXPOSE 443

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]