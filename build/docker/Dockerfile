FROM python:3.10-alpine

ENV PATH=/usr/local/src:$PATH


# update pip
RUN pip3 install --upgrade pip

# install python build dependencies
RUN apk --no-cache add \
    gcc \
    musl-dev \
    libffi-dev \
    make \
    bash \
    openssl \
    curl \
    pcre2-tools \
    grep \
    git \
    jq \
    nano \
    apache2-utils

#install github cli
RUN apk update && \
    apk add --no-cache \
    github-cli \
    openssh-keygen \
    openssh

# install azure cli
RUN pip3 install azure-cli

# install azure devops cli extension
RUN az extension add --name azure-devops


RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh

RUN ssh-keyscan github.com >> /root/.ssh/known_hosts
RUN ssh-keyscan ssh.dev.azure.com >> /root/.ssh/known_hosts

# install yq
RUN curl -L https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -o /usr/local/bin/yq &&\
    chmod +x /usr/local/bin/yq


# install dyff
RUN curl -L https://github.com/homeport/dyff/releases/download/v1.5.8/dyff_1.5.8_linux_amd64.tar.gz -o dyff_v1.5.8_linux_amd64.tar.gz && \
    tar xfv dyff_v1.5.8_linux_amd64.tar.gz -C /usr/local/bin && \
    rm dyff_v1.5.8_linux_amd64.tar.gz

# install helm
RUN curl -L https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz -o helm-v3.12.3-linux-amd64.tar.gz && \
    tar xfv helm-v3.12.3-linux-amd64.tar.gz -C /usr/local/bin && \
    cp /usr/local/bin/linux-amd64/helm /usr/local/bin && \
    rm helm-v3.12.3-linux-amd64.tar.gz

# copy templates
COPY src/templates /usr/local/src/templates

# copy initial config files
COPY src/configs /usr/local/src/configs

# copy bash scriptsx^
COPY src/bash/ /usr/local/bin

RUN chmod +x /usr/local/bin/*

WORKDIR /mnt/pwd

# setup
ENTRYPOINT ["/bin/sh", "-c"]
CMD ["bash"]
#ENTRYPOINT ["tail", "-f", "/dev/null"]