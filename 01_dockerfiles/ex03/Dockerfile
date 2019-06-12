FROM debian

ONBUILD EXPOSE 443 80 22

RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install curl openssh-server ca-certificates postfix tzdata -y

RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

RUN apt-get install -y gitlab-ce

ENTRYPOINT	(/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

# docker build -t ex03 .
# docker run -it --rm -p 8080:80 -p 8443:443 -p 8022:22 --privileged ex03
