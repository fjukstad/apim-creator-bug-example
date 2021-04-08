# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.166.1/containers/ubuntu/.devcontainer/base.Dockerfile

# [Choice] Ubuntu version: bionic, focal
ARG VARIANT="focal"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends make wget vim

WORKDIR /tmp
# Install dotnet (needed for Azure API Management tools)
RUN wget https://download.visualstudio.microsoft.com/download/pr/ab82011d-2549-4e23-a8a9-a2b522a31f27/6e615d6177e49c3e874d05ee3566e8bf/dotnet-sdk-3.1.407-linux-x64.tar.gz
ENV DOTNET_FILE=dotnet-sdk-3.1.407-linux-x64.tar.gz
ENV DOTNET_ROOT=$HOME/dotnet
RUN mkdir -p "$DOTNET_ROOT" && tar zxf "$DOTNET_FILE" -C "$DOTNET_ROOT"
ENV PATH=$PATH:$DOTNET_ROOT

# Install the Creator and Extractor tools used with Azure API Management
USER vscode
RUN git clone https://github.com/Azure/azure-api-management-devops-resource-kit.git
WORKDIR /tmp/azure-api-management-devops-resource-kit/src/APIM_ARMTemplate/apimtemplate
RUN git reset --hard 7c4bff76cc0e0f99f835a4ae27e56c3474fd3907
RUN dotnet pack -c Release
RUN dotnet tool install -g --add-source bin/Release apimtemplate
ENV PATH=$PATH:/home/vscode/.dotnet/tools
