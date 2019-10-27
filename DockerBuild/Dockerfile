FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster
ARG JENKUID
ARG JENKGID
MAINTAINER rowdy1

USER root

RUN groupadd jenkins && \
    groupmod -g $JENKGID jenkins && \
	adduser jenkins && \
	usermod -u $JENKUID jenkins

# run as jenkins user - good practice
USER jenkins
