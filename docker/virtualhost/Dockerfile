ARG PYTHON_VER
FROM python:${PYTHON_VER}

RUN apt update \
    && apt upgrade -y \
    # install packages for development
    && apt -y install --no-install-recommends \
           # NOTE: less enables you to scroll per a line when running 'git diff'
           less vim \
           openssh-server \
           net-tools \
           # For scipy 1.4.1 (is not necessary for scipy 1.7.1)
           # https://stackoverflow.com/questions/14408123/installing-scipy-on-ubuntu/14408210
           libblas-dev liblapack-dev libatlas-base-dev gfortran \
    # Delete cache to minimize the size of this image
    && rm -rf /var/lib/apt/lists/* \
    && apt -y clean \
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

WORKDIR /root/lab
