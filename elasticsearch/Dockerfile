FROM bellsoft/liberica-openjdk-debian:11.0.14.1-1

RUN apt-get update && apt-get -y install sudo curl nano gawk mc && rm -rf /var/lib/apt/lists/*

WORKDIR /app
# надо переделать
COPY 1c_cs appCopy/

# Установка только необходимых компонент
RUN cd /app/appCopy\
	&& ./1ce-installer-cli install 1c-cs --components 1c-cs-elasticsearch@5.6.12-21\
    && chmod +x /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64/ring

# Все переменные сразу прописать в PATH из echo $PATH
ENV PATH /opt/1C/1CE/components/1c-enterprise-ring-0.19.5+12-x86_64:/usr/lib/jvm/jdk-11.0.14.1-bellsoft-x86_64/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p /var/cs/elastic_instance 

EXPOSE 9300

RUN ring elasticsearch instance create --dir /var/cs/elastic_instance --owner root 

ENTRYPOINT ["/opt/1C/1CE/components/1c-cs-elasticsearch-5.6.12-21-x86_64/bin/launcher", "start", "--instance", "/var/cs/elastic_instance", "--javahome", "/usr/lib/jvm/jdk-11.0.14.1-bellsoft-x86_64"]
