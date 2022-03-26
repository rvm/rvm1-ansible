FROM centos:6

RUN sed -i -e 's/mirrorlist/#mirrorlist/g' \
		-e 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' \
		/etc/yum.repos.d/CentOS-* \
	&& yum update -y \
	&& yum install -y \
		sudo \
		which \
	&& yum clean all

RUN useradd -ms /bin/bash user && \
	echo 'user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

CMD ["/usr/bin/env", "sleep", "29m"]
