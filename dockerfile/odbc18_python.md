dockerfile with python 3.11.7 and msodbcsql18
```
# from alpine as base

# ARG app_configuration_conn_str 

# RUN echo $env > /var/env ; echo $app_configuration_conn_str > var/conn_str
  
# builder first step python environment

FROM python:3.11.7-slim-bookworm

# COPY --from=base /var/env /var/env
# COPY --from=base /var/conn_str /var/conn_str

# ENV env DEV

COPY ./requirements.txt /requirements.txt

#apt update & install odbc tools
RUN apt update ; apt install -y curl unixodbc gnupg2 libterm-readline-gnu-perl gpg
RUN curl https://packages.microsoft.com/keys/microsoft.asc > /etc/apt/trusted.gpg.d/microsoft.asc

RUN curl https://packages.microsoft.com/config/debian/12/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN curl -fsSL https://packages.microsoft.com/keys/microsoft.asc |  gpg --dearmor -o /usr/share/keyrings/microsoft-prod.gpg

RUN apt-get update

RUN ACCEPT_EULA=Y apt-get install -y msodbcsql18

RUN echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc

#setting time zone
#pip install packages

RUN cp /usr/share/zoneinfo/Asia/Taipei /etc/localtime ; \
    pip install -r /requirements.txt ;\
    rm /requirements.txt

WORKDIR /



```