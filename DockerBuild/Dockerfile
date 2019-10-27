FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster
ARG JENKUID
ARG JENKGID
MAINTAINER rowdy1

USER root

RUN useradd -u $JENKUID -g $JENKGID jenkins

# run as jenkins user - good practice
USER jenkins
