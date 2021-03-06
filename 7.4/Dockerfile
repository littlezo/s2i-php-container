FROM littleof/s2i-base-centos7

#此镜像提供了一个PHP环境，可用于运行PHP 应用程序。

# Todo 等待有时间在处理

EXPOSE 8080
EXPOSE 8443

ENV NAME=php \
    PHP_VERSION=7.4 \
    PHP_VER_SHORT=74 \
    PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

ENV SUMMARY="Platform for building and running PHP ${PHP_VERSION} applications" \
    DESCRIPTION="PHP ${PHP_VERSION} available as container is a base platform for \
    building and running various PHP ${PHP_VERSION} applications and frameworks. \
    PHP is an HTML-embedded scripting language. PHP attempts to make it easy for developers \
    to write dynamically generated web pages. PHP also offers built-in database integration \
    for several commercial and non-commercial database management systems, so writing \
    a database-enabled webpage with PHP is fairly simple. The most common use of PHP coding \
    is probably as a replacement for CGI scripts."

LABEL summary="$SUMMARY" \
    description="$DESCRIPTION" \
    io.k8s.description="$DESCRIPTION" \
    io.k8s.display-name="PHP ${PHP_VERSION}" \
    io.kubesphere.expose-services="8080:http" \
    io.kubesphere.expose-services="8443:https" \
    io.kubesphere.tags="builder,${NAME},${NAME}${PHP_VERSION}" \
    io.kubesphere.s2i.scripts-url="image:///usr/libexec/s2i" \
    io.s2i.scripts-url="image:///usr/libexec/s2i" \
    io.kubesphere.dev-mode="DEV_MODE:false" \
    io.kubesphere.deployments-dir="${APP_ROOT}/src" \
    com.cloudify.dev-mode.port="DEBUG_PORT:5858"\
    com.cloudify.component="${NAME}${PHP_VER_SHORT}-container" \
    name="littleof/${NAME}-${PHP_VER_SHORT}-centos7" \
    version="${PHP_VERSION}" \
    help="For more information visit https://github.com/littlezo/k8s-php-s2i.git" \
    usage="s2i build https://github.com/littlezo/s2i-php-container.git --context-dir=${PHP_VERSION}/test/test-app littleof/${NAME}-${PHP_VER_SHORT}-centos7 sample-server" \
    maintainer="@小小只^v^ <littlezov@qq.com>" \
    com.label-schema.maintainer-url="https://github.com/littlezo" \
    com.kubesphere.image.title="CentOS Base Image" \
    com.cloudify.image.license="GPL v2.0" \
    com.cloudify.image.created="${BUILD_DATE}"

# 安装 PHP 环境
RUN dnf module enable php:remi-${PHP_VERSION} -y &&\
    dnf --enablerepo=PowerTools install libedit-devel hdf5-devel vips-devel -y &&\
    INSTALL_PKGS="php-gd php-xhprof php-ast php-cli php-dba \
    php-dbg php-pdo php-xml php-imap php-intl php-json \
    php-ldap php-snmp php-soap php-tidy php-devel \
    php-bcmath php-brotli php-common php-recode php-sodium \
    php-xmlrpc php-enchant php-libvirt php-mysqlnd php-pecl-ds \
    php-pecl-ev php-process php-embedded php-mbstring \
    php-pecl-dio php-pecl-eio php-pecl-env php-pecl-lzf \
    php-pecl-nsq php-pecl-psr php-pecl-zip php-pecl-zmq \
    php-componere php-pecl-grpc php-pecl-http php-pecl-ssh2 \
    php-pecl-sync php-pecl-uuid php-pecl-vips \
    php-pecl-yaml php-phpiredis php-wkhtmltox php-pecl-event \
    php-pecl-geoip php-pecl-gnupg php-pecl-mysql php-pecl-oauth \
    php-pecl-stats php-pecl-xattr php-pecl-xxtea php-pecl-base58 \
    php-pecl-hrtime php-pecl-mcrypt php-pecl-pdflib \
    php-pecl-propro php-pecl-redis php-pecl-decimal \
    php-pecl-xmldiff php-pecl-igbinary php-pecl-mogilefs \
    php-pecl-json-post php-pecl-ip2location \
    php-pecl-http-message php-bcmath php-cli php-common php-dba \
    php-dbg php-gd php-gmp php-intl php-json php-ldap \
    php-mbstring php-mysqlnd php-pdo php-process php-pecl-apcu \
    php-recode php-soap php-xml php-zip php-pecl-redis php-swoole \
    php-xmlrpc  php-devel" && \
    yum upgrade -y &&\
    yum install -y libstdc++ openssl pcre-devel pcre2-devel openssl-devel supervisor unzip zlib-devel git wget \
    $INSTALL_PKGS --skip-broken --setopt=tsflags=nodocs --nogpgcheck && \
    yum -y  upgrade --setopt=tsflags=nodocs --nogpgcheck && \
    yum -y clean all --enablerepo='*'

# # Redis 扩展下载 pecl本地安装 开启扩展
# RUN wget http://pecl.php.net/get/redis-${PHPREDIS_VERSION}.tgz -O /tmp/redis.tgz \
#     && pecl install /tmp/redis.tgz \
#     && rm -rf /tmp/redis.tgz

# # Swoole 扩展安装 开启扩展
# RUN wget https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz -O /tmp/swoole.tar.gz \
#     && mkdir -p /tmp/swoole \
#     && tar -xf /tmp/swoole.tar.gz -C /tmp/swoole --strip-components=1 \
#     && rm /tmp/swoole.tar.gz \
#     && ( \
#     cd /tmp/swoole \
#     && phpize \
#     && ./configure --enable-async-redis --enable-openssl --enable-http2 \
#     && make -j$(nproc) \
#     && make install \
#     ) \
#     && rm -r /tmp/swoole
# 安装 Composer
RUN wget -O /usr/local/bin/composer https://mirrors.aliyun.com/composer/composer.phar && \
    chmod a+x /usr/local/bin/composer

ENV APP_DATA=${APP_ROOT} \
    STI_SCRIPTS_URL=image:///usr/libexec/s2i \
    STI_SCRIPTS_PATH=/usr/libexec/s2i \
    S2I_CONFIG_PATH=/app-src/src/deploy.json \
    SCL_ENABLED=php${PHP_VER_SHORT}

ENV PHP_CONTAINER_SCRIPTS_PATH=/usr/share/container-scripts/php \
    BASH_ENV=${APP_ROOT}/etc/scl_enable \
    ENV=${APP_ROOT}/etc/scl_enable \
    PROMPT_COMMAND=". ${APP_ROOT}/etc/scl_enable"

# 拷贝陪着文件
# COPY ./root/app-src/etc/php.d/50-swoole.ini.template  ${PHP_SYSCONF_PATH}/php.d/50-swoole.ini
# COPY ./root/app-src/etc/php.d/99-redis.ini.template  ${PHP_SYSCONF_PATH}/php.d/99-redis.ini

RUN  php -v && php -m && php --ri swoole

COPY ./s2i/bin/ ${STI_SCRIPTS_PATH}

COPY ./root/ /

RUN /usr/libexec/container-setup

RUN chown -R 1001:0 ${APP_ROOT} && chmod -R ug+rwx ${APP_ROOT} && \
    rpm-file-permissions

USER 1001

# Set the default CMD to print the usage of the language image
CMD ${STI_SCRIPTS_PATH}/usage
