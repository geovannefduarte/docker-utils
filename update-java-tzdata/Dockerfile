FROM docker.io/openjdk:12-alpine

RUN apk upgrade --update
RUN apk update && apk add ca-certificates wget
RUN apk add --no-cache tzdata
RUN cp /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
RUN echo "America/Sao_Paulo" >  /etc/timezone
RUN date

RUN echo "Downloading tzupdater"
RUN wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/tzupdater/2.3.2/tzupdater-2.3.2.zip
RUN unzip tzupdater-2.3.2.zip

RUN echo "Downloading latest tzdata"
RUN wget -O /tmp/tzdata-latest.tar.gz http://www.iana.org/time-zones/repository/tzdata-latest.tar.gz
RUN sha512sum /tmp/tzdata-latest.tar.gz | cut -c -128 > /tmp/tzdata-latest.tar.gz.sha512

RUN echo "Apply update"
RUN cd tzupdater-2.3.2 && ls -l && java -jar tzupdater.jar -v -u -l file:/tmp/tzdata-latest.tar.gz

ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
