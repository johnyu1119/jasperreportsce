FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com> 20210716a"
# version 20210716a
#USER root
#ENV BITNAMI_PKG_CHMOD="-R g+rwX"

ENV BITNAMI_PKG_CHMOD="-R g+rwX" \
    BITNAMI_PKG_EXTRA_DIRS="/tmp/bitnami /.nami /opt/bitnami/java/bin /opt/bitnami/tomcat/bin /opt/bitnami/tomcat/webapps /opt/bitnami/tomcat/webapps/jasperserver /opt/bitnami/mysql/bin /opt/bitnami/jasperreports /opt/bitnami/jasperreports/WEB-INF /opt/bitnami/jasperreports/WEB-INF/logs /opt/bitnami/git/bin /opt/bitnami/common/bin /opt/bitnami/nami/bin" \
    HOME="/"

ENV PATH="/opt/bitnami/jasperreports/WEB-INF/logs:/opt/bitnami/java/bin:/opt/bitnami/tomcat/bin:/opt/bitnami/mysql/bin:/opt/bitnami/git/bin:/opt/bitnami/common/bin:/opt/bitnami/nami/bin:$PATH"

COPY /7/debian-10/prebuildfs /


RUN apt-get update
RUN apt-get install vim -y
# Install required system packages and dependencies
RUN install_packages ca-certificates curl fontconfig gzip libaudit1 libc6 libcap-ng0 libcom-err2 libcurl4 libffi6 libfreetype6 libgcc1 libgcrypt20 libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libncurses6 libnettle6 libnghttp2-14 libp11-kit0 libpam0g libpsl5 librtmp1 libsasl2-2 libssh2-1 libssl1.1 libstdc++6 libtasn1-6 libtinfo6 libunistring2 libxml2 procps sudo tar zlib1g
#RUN install_packages curl fontconfig gzip libaudit1 libc6 libcap-ng0 libcom-err2 libcurl4 libffi6 libfreetype6 libgcc1 libgcrypt20 libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libncurses6 libnettle6 libnghttp2-14 libp11-kit0 libpam0g libpsl5 librtmp1 libsasl2-2 libssh2-1 libssl1.1 libstdc++6 libtasn1-6 libtinfo6 libunistring2 libxml2 procps sudo tar zlib1g
RUN /build/bitnami-user.sh
RUN /build/install-nami.sh
RUN bitnami-pkg install java-1.8.292-0 --checksum 700e2d8391934048faefb45b4c3a2af74bc7b85d4c4e0e9a24164d7256456ca2
RUN bitnami-pkg unpack tomcat-9.0.46-1 --checksum 73545d679fa3975841e05f1a8a3d72258fddaeb67ccc28f38fe290fd71772e0d
RUN bitnami-pkg unpack mysql-client-10.3.29-0 --checksum 5bf8f1ed022c8ad75a4db5b8b72ae54ce427bff628d7e4025c5b5e67b876708d
RUN bitnami-pkg install git-2.32.0-0 --checksum 3094f50c904e966bd6c6a2d6152cf41fd3a816d32395622a785ccbcda62b7c5f
RUN bitnami-pkg install tini-0.19.0-1 --checksum 9b1f1c095944bac88a62c1b63f3bff1bb123aa7ccd371c908c0e5b41cec2528d

RUN useradd -ms /bin/bash -u 1001 tomcat -G root
#WORKDIR /root
RUN ls -al

#RUN mv /root/.nami /.nami
RUN usermod -a -G root tomcat
#     chown -R tomcat /opt/bitnami/tomcat /bitnami /opt/bitnami/jasperreports/WEB-INF/logs
#USER root

RUN bitnami-pkg unpack jasperreports-7.8.0-6
RUN bitnami-pkg install gosu-1.13.0-0 --checksum fd7257c2736164d02832dbf72e2c1ed9d875bf3e32f0988520796bc503330129

COPY /7/debian-10/rootfs /
ENV ALLOW_EMPTY_PASSWORD="no" \
    BITNAMI_APP_NAME="jasperreports" \
    BITNAMI_IMAGE_VERSION="7.8.0-debian-10-r240" \
    JASPERREPORTS_DATABASE_NAME="bitnami_jasperreports" \
    JASPERREPORTS_DATABASE_PASSWORD="" \
    JASPERREPORTS_DATABASE_USER="bn_jasperreports" \
    JASPERREPORTS_EMAIL="user@example.com" \
    JASPERREPORTS_PASSWORD="bitnami" \
    JASPERREPORTS_USERNAME="user" \
    MARIADB_HOST="mariadb" \
    MARIADB_PORT_NUMBER="3306" \
    MARIADB_ROOT_PASSWORD="" \
    MARIADB_ROOT_USER="root" \
    MYSQL_CLIENT_CREATE_DATABASE_NAME="" \
    MYSQL_CLIENT_CREATE_DATABASE_PASSWORD="" \
    MYSQL_CLIENT_CREATE_DATABASE_PRIVILEGES="ALL" \
    MYSQL_CLIENT_CREATE_DATABASE_USER="" \
    MYSQL_CLIENT_ENABLE_SSL="no" \
    MYSQL_CLIENT_SSL_CA_FILE="" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux" \
    SMTP_EMAIL="" \
    SMTP_HOST="" \
    SMTP_PASSWORD="" \
    SMTP_PORT="" \
    SMTP_PROTOCOL="" \
    SMTP_USER=""

EXPOSE 8080 8443

ENTRYPOINT [ "/app-entrypoint.sh" ]


# for openshift non-root account
#USER root
#ENV NAMI_PREFIX="/opt/bitnami/nami"

#RUN useradd -ms /bin/bash -u 1001 tomcat 

#RUN echo $(ls)
#mv /root/.nami $NAMI_PREFIX && \
#mkdir /bitnami && \
#RUN chown -R 1001 $NAMI_PREFIX /opt/bitnami/tomcat /opt/bitnami/git

#WORKDIR /opt/bitnami/jasperreports/WEB-INF/logs
#RUN ls -al

#RUN echo 'line13'

# Temporary Test
#RUN echo $(ls)

USER 1001

CMD [ "nami", "start", "--foreground", "tomcat" ]
