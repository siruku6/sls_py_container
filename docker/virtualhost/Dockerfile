FROM node:18.12.1-buster-slim as node

FROM python:3.9.10

# Copy node / npm to base image from node image
COPY --from=node /usr/local/bin/ /usr/local/bin/
COPY --from=node /usr/local/lib/node_modules /usr/local/lib/node_modules

RUN apt update \
    && apt upgrade -y \
    # install packages for development
    && apt -y install --no-install-recommends \
           # NOTE: less enables you to scroll per a line when running 'git diff'
           less vim \
           openssh-server \
           net-tools \
           zip \
    # Delete cache to minimize the size of this image
    && rm -rf /var/lib/apt/lists/* \
    && apt -y clean \
    # for install aws-cli
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip && rm awscliv2.zip && ./aws/install \
    # for install serverless
    && npm install -g npm \
    && npm install -g serverless@3 \
    # for updating python module
    && pip install --upgrade pip \
    && pip install pipenv

RUN { \
    echo "alias g='git'"; \
    echo "alias ls='ls $LS_OPTIONS --color=auto'"; \
    echo "alias ll='ls -alF'"; \
    echo "alias la='ls -A'"; \
    echo "alias l='ls -CF'"; \
    echo "alias grep='grep --color=auto'"; \
    echo "alias fgrep='fgrep --color=auto'"; \
    echo "alias egrep='egrep --color=auto'"; \
} >> ~/.bashrc
RUN . ~/.bashrc

# for ssh login: The opened port No is ${SSH_PORT}
ARG SSH_PORT
RUN { \
    mkdir /var/run/sshd; \
    echo 'root:password' | chpasswd; \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd; \
    sed -i '/pam_loginuid\.so/s/required/optional/' /etc/pam.d/sshd; \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config; \
    sed -i "s/#Port 22/Port ${SSH_PORT}/" /etc/ssh/sshd_config; \
}

ARG PERSONAL_ACCESS_TOKEN
RUN if [ "${PERSONAL_ACCESS_TOKEN}" ]; \
    then \
        touch /root/.netrc; \
        echo 'machine github.com' >> /root/.netrc; \
        echo "  login ${PERSONAL_ACCESS_TOKEN}" >> /root/.netrc; \
        echo '  password x-oauth-basic' >> /root/.netrc; \
    fi
