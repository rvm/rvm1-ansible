FROM debian:9

RUN useradd -ms /bin/bash user

RUN apt-get update && \
    apt-get install -y \
		curl \
		gpg \
		python \
		sudo \
		systemd \
		&& rm -rf /var/lib/apt/lists/*

CMD ["/bin/systemd"]
