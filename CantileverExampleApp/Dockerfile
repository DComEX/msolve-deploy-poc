# Base image to be published
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
LABEL author=”MGroup”
WORKDIR /CantileverExampleApp

# Build .NET app
FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /CantileverExampleApp
COPY CantileverExampleDocker.Docker.csproj .
RUN dotnet restore
COPY . .
RUN dotnet build -c Release

# Publish .NET app
FROM build AS publish
RUN dotnet publish -c Release -o /publish

# Final image with entrypoint
FROM base AS final
WORKDIR /CantileverExampleApp
COPY --from=publish /publish .
# MPI Dependencies
RUN apt-get -y update
RUN apt-get -y install git
RUN mkdir ~/src && cd ~/src
RUN git clone https://github.com/jmp75/MPI.NET.git mpi.net
RUN apt-get -y install build-essential
RUN apt-get -y install dirmngr gnupg apt-transport-https ca-certificates software-properties-common
RUN apt-get -y install mono-complete
RUN apt-get -y install openmpi-bin libopenmpi-dev
RUN apt-get -y install lmod
RUN export PATH=/usr/share/lmod/6.6/libexec:$PATH
# RUN "source /usr/share/lmod/6.6/init/bash"
# RUN "export LMOD_CMD=/usr/share/lmod/6.6/libexec/lmod"
# ENTRYPOINT ["dotnet", "CantileverExampleDocker.Docker.dll"]