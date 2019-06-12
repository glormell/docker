FROM debian

EXPOSE 9987/udp 30033 10011

RUN apt-get update && apt-get upgrade && apt-get install curl bzip2 -y && \
curl https://files.teamspeak-services.com/releases/server/3.8.0/teamspeak3-server_linux_amd64-3.8.0.tar.bz2 > ts && tar xvf ts && rm -rf ts

CMD teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh license_accepted=1
