FROM    ubuntu:16.04
MAINTAINER      yourname "yourname@site.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd cg
RUN     /bin/echo 'cg:123456' |chpasswd
RUN     apt-get update && apt-get install -y gcc
EXPOSE  80
USER    cg
