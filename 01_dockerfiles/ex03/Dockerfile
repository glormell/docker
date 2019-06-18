FROM debian

ONBUILD EXPOSE 443 80 22

RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install curl openssh-server ca-certificates postfix tzdata -y

RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

RUN apt-get install -y gitlab-ce

RUN echo "letsencrypt['enable'] = false" > /etc/gitlab/gitlab.rb \
	&& echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/gitlab.example.com.crt\"" >> /etc/gitlab/gitlab.rb \
	&& echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/gitlab.example.com.key\"" >> /etc/gitlab/gitlab.rb \
	&& echo "external_url 'https://gitlab.example.com'" >> /etc/gitlab/gitlab.rb

RUN openssl req \
	-subj "/C=US/ST=Denial/L=Springfield/O=Dis/CN=www.example.com" \
	-newkey rsa:2048 -nodes -keyout gitlab.example.com.key \
	-x509 -days 365 -out gitlab.example.com.crt

RUN mkdir -p /etc/gitlab/ssl && chmod 700 /etc/gitlab/ssl && cp gitlab.example.com.key gitlab.example.com.crt /etc/gitlab/ssl/

RUN mkdir -p /opt/gitlab/sv/sshd/supervise \
	&& mkfifo /opt/gitlab/sv/sshd/supervise/ok \
	&& printf "#!/bin/sh\nexec 2>&1\numask 077\nexec /usr/sbin/sshd -D" > /opt/gitlab/sv/sshd/run \
	&& chmod a+x /opt/gitlab/sv/sshd/run \
	&& ln -s /opt/gitlab/sv/sshd /opt/gitlab/service \
	&& mkdir -p /var/run/sshd

ENTRYPOINT	(/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

# docker build -t ex03 .
# docker run -it --rm -p 8080:80 -p 8443:443 -p 8022:22 --privileged ex03
