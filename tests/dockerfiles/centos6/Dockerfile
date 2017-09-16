FROM centos:6

RUN useradd -ms /bin/bash user

RUN yum update -y && \
		yum install -y \
			initscripts \
			sudo \
		&& yum clean all

CMD ["/sbin/init"]
