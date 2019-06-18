FROM debian

ONBUILD EXPOSE 1450/udp

RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install git perl -y

RUN git clone https://github.com/nickaroot/perl-udp-client-server.git server

RUN mkdir ~/LOG && touch ~/LOG/servupdlog

ENTRYPOINT cd server && ./n_serv 1450 && tail -f /dev/null

# docker build -t bonus .
# docker run -it --rm -p 1450:1450/udp --privileged bonus
# ./n_cln 1450
