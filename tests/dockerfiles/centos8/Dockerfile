FROM centos:8

RUN sed -i -e 's/mirrorlist/#mirrorlist/g' \
		-e 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' \
		/etc/yum.repos.d/CentOS-* \
	&& dnf update -y \
	&& dnf install -y \
		sudo \
		which \
	&& dnf clean all

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/usr/sbin/init"]
