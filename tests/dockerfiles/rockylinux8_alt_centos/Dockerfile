FROM rockylinux:8

RUN dnf update -y \
	&& dnf install -y \
		procps \
		sudo \
		which \
	&& dnf clean all

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/usr/sbin/init"]
