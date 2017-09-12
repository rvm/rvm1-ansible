FROM ubuntu:16.04

RUN useradd -ms /bin/bash user

RUN apt-get update && \
    apt-get install -y \
		curl \
		python \
		sudo \
		&& rm -rf /var/lib/apt/lists/*

CMD ["/sbin/init"]
