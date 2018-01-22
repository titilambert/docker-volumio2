# STAGE 1: build s6

FROM ubuntu:artful-20171116

RUN \
	echo force-unsafe-io > /etc/dpkg/dpkg.cfg.d/disable-fsync && \
	apt-get update && \
	apt-get install -y eatmydata && \
	dpkg -L eatmydata libeatmydata1 | grep 'libeatmydata.so$' > /etc/ld.so.preload && \ 
	eatmydata apt-get install -y autoconf automake build-essential curl git-core patch wget musl-tools

COPY build-s6 ${BUILD_DIR}/
RUN DIST_DIR=/dist ${BUILD_DIR}/build-s6


# STAGE 2: build image

FROM ubuntu:artful-20171116

ENV DEBIAN_FRONTEND=noninteractive LANG=en_US.UTF-8 TERM=xterm

RUN \
	echo 'deb http://archive.ubuntu.com/ubuntu/ artful main restricted universe multiverse' > /etc/apt/sources.list && \
	echo 'deb http://archive.ubuntu.com/ubuntu/ artful-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
	echo 'path-exclude=/usr/share/man/*' > /etc/dpkg/dpkg.cfg.d/docker-no-doc && \
	echo 'path-exclude=/usr/share/doc/*' >> /etc/dpkg/dpkg.cfg.d/docker-no-doc && \
	echo 'path-exclude=/usr/share/locale/*' >> /etc/dpkg/dpkg.cfg.d/docker-no-doc && \
	rm -rf /usr/share/man/ /usr/share/doc/ /usr/share/locale/ && \
	echo force-unsafe-io > /etc/dpkg/dpkg.cfg.d/disable-fsync && \
	apt-get update && \
	LANG=C apt-get install -y --no-install-recommends eatmydata locales apt-utils && \
	locale-gen en_US.UTF-8 && \
	update-locale && \
	eatmydata apt-get install -y --no-install-recommends \
		apt-transport-https \
		bzip2 \
		ca-certificates \
		curl \
		dnsutils \
		file \
		git-core \
		gosu \
		htop \
		iputils-ping \
		netcat-openbsd \
		net-tools \
		openssl \
		procps \
		rsync \
		socklog \
		software-properties-common \
		strace \
		sysstat \
		tcpdump \
		telnet \
		vim \
		unzip \
        minizip \
		zip \
	&& \
	curl -L -o /usr/bin/tini https://github.com/krallin/tini/releases/download/v0.16.1/tini-static && \
	chmod +x /usr/bin/tini && \
	mkdir -p /etc/startup/ /etc/service/ && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
	rm -f rm -f /var/cache/debconf/config.dat-old /var/cache/debconf/templates.dat-old && \
	find /var/log -type f -delete

COPY --from=0 /dist /
RUN \
	mkdir -p /usr/lib/s6/shutdown.d /etc/service/.s6-svscan && \
	for signal in SIGINT SIGHUP SIGTERM SIGQUIT SIGUSR1 SIGUSR2; \
		do ln -s /usr/bin/s6-shutdown /etc/service/.s6-svscan/$signal; done
COPY . /

ENTRYPOINT ["/usr/bin/entrypoint"]
