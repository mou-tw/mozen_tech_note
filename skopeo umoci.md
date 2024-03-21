```

FROM ubuntu:latest

RUN apt update
RUN apt install iproute2 inetutils-ping curl -y
# install arp
RUN apt-get install net-tools


ENTRYPOINT ["bash"]

```