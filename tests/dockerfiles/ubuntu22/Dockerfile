FROM ubuntu:22.04

RUN apt-get update \
	&& apt-get install -y \
		curl \
		gpg \
		python3 \
		sudo \
		systemd \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/usr/bin/sleep", "infinity"]
