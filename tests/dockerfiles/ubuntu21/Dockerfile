FROM ubuntu:21.10

RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install \
		-y --no-install-recommends \
		ca-certificates \
		curl \
		dirmngr \
		gpg \
		python \
		sudo \
		systemd \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/usr/bin/env", "sleep", "29m"]
