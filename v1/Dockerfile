# 基础镜像
FROM ubuntu:18.04

# 作者信息
MAINTAINER tinywan@163.com

# 设置环境变量
ENV PHP_VERSION 7.3.0
# 报错debconf: unable to initialize frontend: Dialog
ENV DEBIAN_FRONTEND noninteractive

# 复制命令，把本机的文件复制到镜像中。提前都放进基础镜像的/usr/local/src目录下，方便编译安装
# ADD php-7.0.0.tar.gz /usr/local/src
# 更新源sources.list 
ADD ./sources.list /etc/apt 

# 安装依赖
# RUN apt-get update && apt-get install --assume-yes apt-utils
RUN apt-get update && apt-get install --assume-yes apt-utils
RUN apt-get upgrade -y
RUN set -x \
    && apt-get install -y libkrb5-dev wget \
        libc-client2007e     \
        libc-client2007e-dev \
        libcurl4-openssl-dev \
        libbz2-dev           \
        libjpeg-dev          \
        libmcrypt-dev        \
        libxslt1-dev         \
        libxslt1.1           \
        libpq-dev            \
        libfreetype6-dev     \
        libzip-dev           \
        libpng-dev           \
        build-essential      \
        git                  \
        make \
        bison \
        re2c \
        vim \
        vi \
    # 建立目录
    && mkdir ~/download \
    && cd ~/download \   
    # 下载源码
    && wget http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
    && tar -zxf php-$PHP_VERSION.tar.gz \
    && cd php-$PHP_VERSION \
    # && groupadd -f www-data \
    # && useradd -r -g www-data -s /bin/bash www-data \ 
    # 检查配置文件
    && ./configure \
        --prefix=/usr/local/php \
        --with-config-file-path=/usr/local/php/etc \
        --with-config-file-scan-dir=/usr/local/php/conf.d/ \
        --with-zlib-dir \
        --with-freetype-dir \
        --enable-mbstring                            \
        --with-libxml-dir=/usr                       \
        --enable-soap                                \
        --enable-calendar                            \
        --with-curl                                  \
        --with-zlib                                  \
        --with-gd                                    \
        --disable-rpath                              \
        --enable-inline-optimization                 \
        --with-bz2                                   \
        --with-zlib                                  \
        --enable-sockets                             \
        --enable-sysvsem                             \
        --enable-sysvshm                             \
        --enable-pcntl                               \
        --enable-mbregex                             \
        --enable-exif                                \
        --enable-bcmath                              \
        --with-mhash                                 \
        --enable-zip                                 \
        --with-pcre-regex                            \
        --with-pdo-mysql                             \
        --with-mysqli                                \
        --with-mysql-sock=/var/run/mysqld/mysqld.sock \
        --with-jpeg-dir=/usr                         \
        --with-png-dir=/usr                          \
        --with-openssl                               \
        --with-fpm-user=www-data                     \
        --with-fpm-group=www-data                    \
        --enable-ftp                                 \
        --with-imap                                  \
        --with-imap-ssl                              \
        --with-kerberos                              \
        --with-gettext                               \
        --with-xmlrpc                                \
        --with-xsl                                   \
        --enable-opcache                             \
        --enable-fpm    \
    # 编译安装    
    && make \
    && make install \
    # 复制配置文件
    && cp ~/download/php-$PHP_VERSION/php.ini-production /usr/local/php/etc/php.ini \
    && cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf \
    && cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf \
    # 加入环境变量
    && echo "export PATH=$PATH:/usr/local/php/bin" >> /etc/profile \
    && . /etc/profile \
    # 删除安装文件
    && rm -rf ~/download \
    && apt-get clean 

# 设置容器启动时要运行的命令只有在你执行 docker run 或者 docker start 命令是才会运行，其他情况下不运行。
CMD ["/usr/local/php/sbin/php-fpm"]

# 设置暴露端口号，注意是容器暴露端口号，并不是暴露到物理机上的端口号
EXPOSE 9000