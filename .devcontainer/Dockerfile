# Update the VARIANT arg in docker-compose.yml to pick a Node version: 10, 12, 14
ARG VARIANT=12
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-${VARIANT}

# Install MongoDB command line tools
ARG MONGO_TOOLS_VERSION=4.2
RUN curl -sSL "https://www.mongodb.org/static/pgp/server-${MONGO_TOOLS_VERSION}.asc" | (OUT=$(apt-key add - 2>&1) || echo $OUT) \
    && echo "deb http://repo.mongodb.org/apt/debian $(lsb_release -cs)/mongodb-org/${MONGO_TOOLS_VERSION} main" | tee /etc/apt/sources.list.d/mongodb-org-${MONGO_TOOLS_VERSION}.list \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y mongodb-org-tools mongodb-org-shell \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Update args in docker-compose.yaml to set the UID/GID of the "node" user.
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then groupmod --gid $USER_GID node && usermod --uid $USER_UID --gid $USER_GID node; fi

ARG LOCAL=/usr/local
ARG HOME=/home/node
ARG PLATFORM=/workspace/platform

# Installing Go Lang
ARG GO_VERSION=1.16.3
RUN wget https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz \
    && tar -C ${LOCAL} -xzf go${GO_VERSION}.linux-amd64.tar.gz \
    && rm go${GO_VERSION}.linux-amd64.tar.gz
ENV GOPATH ${HOME}/go
ENV PATH $PATH:${LOCAL}/go/bin:$GOPATH/bin

# Installing Protocol Buffer Compiler
ARG PROTOC_VERSION=3.15.8
RUN wget https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOC_VERSION}/protoc-${PROTOC_VERSION}-linux-x86_64.zip \
    && sudo unzip protoc-${PROTOC_VERSION}-linux-x86_64.zip -d ${LOCAL} bin/protoc \
    && sudo unzip -o protoc-${PROTOC_VERSION}-linux-x86_64.zip -d ${LOCAL}/.local/ include/* \
    && rm protoc-${PROTOC_VERSION}-linux-x86_64.zip
# ENV PROTOC_PATH ${HOME}/go    
# ENV PATH $PATH:${HOME}/.local/bin/protoc

# Installing EVANs CLI
ARG EVANS_CLI_VERSION=0.9.3
RUN wget https://github.com/ktr0731/evans/releases/download/${EVANS_CLI_VERSION}/evans_linux_amd64.tar.gz \
    && tar -C /usr/local/bin/ -xzf evans_linux_amd64.tar.gz \
    && rm evans_linux_amd64.tar.gz

# Installing Protocol Buffer Go Plugin
ARG PROTOC_GEN_GO_VERSION=1.26.0
RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@v${PROTOC_GEN_GO_VERSION}
ENV PATH $PATH:${HOME}/go/bin/protoc-gen-go

ARG PROTOC_GEN_GO_GRPC_VERSION=1.1.0
RUN go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v${PROTOC_GEN_GO_GRPC_VERSION}
ENV PATH $PATH:${HOME}/go/bin/protoc-gen-go-grpc

RUN go get google.golang.org/grpc
RUN go get go.mongodb.org/mongo-driver/mongo

ENV PATH $PATH:$(go env GOPATH)/bin

# Exporting path for all go libraries.
ENV PATH $PATH:${HOME}/go/bin

# 
RUN sudo chown -R node:node /home/node/

# Additional libraries and frameworks
RUN su node -c "npm install -g typescript"
RUN su node -c "npm install -g firebase-tools"
RUN su node -c "npm install -g @angular/cli"
RUN su node -c "npm install -g @nestjs/cli"

# [Optional] Uncomment this section to install additional OS packages.

# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
# Install JAVA
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    openjdk-11-jre

# Prints installed java version, just for checking
RUN java --version

# Custom Environment Variables
ENV FIRESTORE_EMULATOR_HOST=localhost:9002

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# [Optional] Uncomment if you want to install more global node modules
# RUN su node -c "npm install -g <your-package-list-here>"






