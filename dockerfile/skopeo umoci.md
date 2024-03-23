```

FROM ubuntu:latest

RUN apt update
RUN apt install iproute2 inetutils-ping curl -y
# install arp
RUN apt-get install net-tools


ENTRYPOINT ["bash"]

```



```
FROM alpine:latest

RUN apk update
RUN apk upgrade

#install packages
RUN apk add curl skopeo umoci nano 


ENTRYPOINT ["sh"]


```