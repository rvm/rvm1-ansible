FROM centos:7

RUN useradd -ms /bin/bash user

RUN yum update -y && \
    yum install -y \
			sudo \
			which \
		&& yum clean all

CMD ["/usr/sbin/init"]
