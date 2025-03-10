FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV BITNAMI_PKG_EXTRA_DIRS="/opt/bitnami/airflow/dags" \
    HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libbsd0 libbz2-1.0 libc6 libcom-err2 libcurl4 libedit2 libffi6 libgcc1 libgcrypt20 libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libmariadb3 libncursesw6 libnettle6 libnghttp2-14 libp11-kit0 libpsl5 libreadline7 librtmp1 libsasl2-2 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libtasn1-6 libtinfo6 libunistring2 libuuid1 libxml2 libxslt1.1 locales netbase procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-7" --checksum d6280b6f647a62bf6edc74dc8e526bfff63ddd8067dcb8540843f47203d9ccf1
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "wait-for-port" "1.0.3-0" --checksum 0603c8eaf6d24e76563431e36e512da06bfebb3a06ede31b3e84d9879213c162
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "python" "3.8.13-3" --checksum a328c8fb3db9e60d3aa19eb7ca31de5da372affcb3d7c0d73610b4a19b634f94
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "postgresql-client" "10.21.0-0" --checksum d5820263d74b7847f65a05b513f1e1ab196128cd9e3dff1b90a2063276667982
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "ini-file" "1.4.3-0" --checksum 2c9ca79a0586a4ec64c66aceda27ebab1ea877dc63982203b5eb71150e00a5e9
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "git" "2.36.1-0" --checksum e00f740b66faf2808cb8983a39b186d43a8367730512c402df78751a9cefbfc1
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "airflow" "2.3.0-2" --checksum e28bd0817b56b9faba9097696094403685342f5fbbbf384bf71784f042dc752c
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN localedef -c -f UTF-8 -i en_US en_US.UTF-8
RUN update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX && \
    DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales
RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && locale-gen

COPY rootfs /
RUN /opt/bitnami/scripts/airflow/postunpack.sh
RUN /opt/bitnami/scripts/locales/add-extra-locales.sh
ENV AIRFLOW_HOME="/opt/bitnami/airflow" \
    APP_VERSION="2.3.0" \
    BITNAMI_APP_NAME="airflow" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LD_LIBRARY_PATH="/opt/bitnami/python/lib/:/opt/bitnami/airflow/venv/lib/python3.8/site-packages/numpy.libs/:$LD_LIBRARY_PATH" \
    LIBNSS_WRAPPER_PATH="/opt/bitnami/common/lib/libnss_wrapper.so" \
    LNAME="airflow" \
    NSS_WRAPPER_GROUP="/opt/bitnami/airflow/nss_group" \
    NSS_WRAPPER_PASSWD="/opt/bitnami/airflow/nss_passwd" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/python/bin:/opt/bitnami/postgresql/bin:/opt/bitnami/git/bin:/opt/bitnami/airflow/venv/bin:$PATH"

EXPOSE 8080

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/airflow/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/airflow/run.sh" ]
