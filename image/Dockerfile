
#FROM ubuntu:bionic-20210222
FROM ubuntu:jammy-20211029

ARG TERRAFORM_VERSION="1.1.4"
#ARG ANSIBLE_VERSION="2.13.1"
ARG PACKER_VERSION="1.7.3"
ARG AWSCLI_VERSION="1.19.27"

LABEL maintainer="Brandon Wilkins <brandon.wilkins@kinandcarta.com>"
LABEL terraform_version=${TERRAFORM_VERSION}
LABEL ansible_version=${ANSIBLE_VERSION}
LABEL aws_cli_version=${AWSCLI_VERSION}

ENV DEBIAN_FRONTEND=noninteractive
ENV TERRAFORM_VERSION=${TERRAFORM_VERSION}
ENV AWSCLI_VERSION=${AWSCLI_VERSION}

RUN apt-get update \
    && apt-get install -y ansible curl python3 python3-pip python3-boto unzip ssh less groff jq \
    git ca-certificates gnupg lsb-release apt-transport-https software-properties-common \
    && pip3 install --upgrade awscli==${AWSCLI_VERSION} \
    && curl -LO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && unzip '*.zip' -d /usr/local/bin \
    && rm *.zip

# Continue Configuring Docker
RUN mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && chmod a+r /etc/apt/keyrings/docker.gpg \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" \
    && apt-get update \
    && apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN ln -s /usr/bin/python3 /usr/bin/python

WORKDIR /opt/app

VOLUME ["/opt/app"]

CMD ["/bin/bash"]
