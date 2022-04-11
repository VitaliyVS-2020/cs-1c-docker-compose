FROM bellsoft/liberica-openjdk-debian:11.0.14.1-1

RUN apt-get update && apt-get -y install sudo curl nano gawk mc && rm -rf /var/lib/apt/lists/*

WORKDIR /app
# надо переделать
COPY 1c_cs appCopy/

# Установка только необходимых компонент
RUN cd /app/appCopy\
	&& ./1ce-installer-cli install 1c-cs --components 1c-cs-server-small@12.0.24\
    && chmod +x /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64/ring

# Все переменные сразу прописать в PATH из echo $PATH
ENV PATH /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64:/usr/lib/jvm/jdk-11.0.14.1-bellsoft-x86_64/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Директория для инстанса
RUN mkdir -p /var/cs/cs_instance

EXPOSE 8087
EXPOSE 8086

# Создать инстанс
RUN ring cs instance create --dir /var/cs/cs_instance --owner root 

# Настроить cs на работу с компонентами из других контейнеров
    # "внешний" hazelcast
RUN ring cs --instance cs_instance hazelcast set-params --group-name 1ce-cs --group-password cs-pass --addresses hazelcast\
    # "внешний" elasticsearch
    && ring cs --instance cs_instance elasticsearch set-params --addresses elasticsearch:9300\
    # "внешний" db
    && ring cs --instance cs_instance jdbc pools --name common set-params --url jdbc:postgresql://db:5432/cs_db?currentSchema=public\
    && ring cs --instance cs_instance jdbc pools --name common set-params --username postgres\
    && ring cs --instance cs_instance jdbc pools --name common set-params --password postgres\
    && ring cs --instance cs_instance jdbc pools --name privileged set-params --url jdbc:postgresql://db:5432/cs_db?currentSchema=public\
    && ring cs --instance cs_instance jdbc pools --name privileged set-params --username postgres\
    && ring cs --instance cs_instance jdbc pools --name privileged set-params --password postgres\
    # WebSocket
    && ring cs --instance cs_instance websocket set-params --hostname cs\
    # порт не 8087, т.к. это административный интерфейс
    && ring cs --instance cs_instance websocket set-params --port 8086

ENTRYPOINT ["/opt/1C/1CE/components/1c-cs-server-small-12.0.24-x86_64/bin/launcher", "start", "--instance", "/var/cs/cs_instance", "--javahome", "/usr/lib/jvm/jdk-11.0.14.1-bellsoft-x86_64"]
