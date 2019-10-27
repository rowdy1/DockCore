FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster
ARG JENKUID
ARG JENKGID
MAINTAINER rowdy1

USER root

RUN useradd jenkins && \
    groupmod -g $JENKGID jenkins && \
    usermod -u $JENKUID jenkins && \
	mkdir /app && \
	chown -R jenkins:jenkins /app
	

# run as jenkins user - good practice
USER jenkins
