FROM ubuntu:16.04 
ENV ZK_USER=zookeeper \
ZK_DATA_DIR=/var/lib/zookeeper/data \
ZK_DATA_LOG_DIR=/var/lib/zookeeper/log \
ZK_LOG_DIR=/var/log/zookeeper \
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ARG version=3.6.2
RUN set -x \
    && apt-get update \
    && apt-get install -y openjdk-8-jre-headless  net-tools wget netcat-openbsd \
	&& wget -q "https://downloads.apache.org/zookeeper/zookeeper-$version/apache-zookeeper-$version-bin.tar.gz" \
    && wget -q "https://downloads.apache.org/zookeeper/zookeeper-$version/apache-zookeeper-$version-bin.tar.gz.asc" \
    && tar -xzf "apache-zookeeper-$version-bin.tar.gz" -C /opt \
    && rm -r "apache-zookeeper-$version-bin.tar.gz" "apache-zookeeper-$version-bin.tar.gz.asc" \
    && mv /opt/apache-zookeeper-3.6.2-bin /opt/zookeeper && rm -rf /opt/zookeeper/LICENSE.txt  /opt/zookeeper/NOTICE.txt  /opt/zookeeper/README.md /opt/zookeeper/README_packaging.md \
	&& apt-get autoremove -y wget \
	&& rm -rf /var/lib/apt/lists/*

#Copy configuration generator script to bin
COPY scripts /opt/zookeeper/bin/

# Create a user for the zookeeper process and configure file system ownership 
# for nessecary directories and symlink the distribution as a user executable
RUN set -x \
	&& useradd $ZK_USER \
    && [ `id -u $ZK_USER` -eq 1000 ] \
    && [ `id -g $ZK_USER` -eq 1000 ] \
    && mkdir -p $ZK_DATA_DIR $ZK_DATA_LOG_DIR $ZK_LOG_DIR /usr/share/zookeeper /tmp/zookeeper /usr/etc/ \
	&& chown -R "$ZK_USER:$ZK_USER" /opt/zookeeper $ZK_DATA_DIR $ZK_LOG_DIR $ZK_DATA_LOG_DIR /tmp/zookeeper \
	&& ln -s /opt/zookeeper/conf/ /usr/etc/zookeeper \
	&& ln -s /opt/zookeeper/bin/* /usr/bin \
	&& ln -s /opt/zookeeper/lib/* /usr/share/zookeeper 