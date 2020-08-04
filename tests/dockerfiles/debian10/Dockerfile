FROM debian:10

RUN apt-get update \
	&& apt-get install -y \
		curl \
		gpg \
		procps \
		python \
		sudo \
		systemd \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/lib/systemd/systemd"]
