FROM debian:8

WORKDIR /opt/

RUN apt-get update && \
    apt-get install -y \
		curl \
	  build-essential \
		libbz2-dev \ 
		libffi-dev \
		libncurses5-dev \
		libreadline-dev \ 
		libssl-dev \ 
		sudo \
		wget \
		&& rm -rf /var/lib/apt/lists/*

COPY build /opt/build
RUN  bash build

CMD ["/sbin/init"]
