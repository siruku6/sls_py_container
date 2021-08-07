ARG PYTHON_VER
FROM python:${PYTHON_VER}

RUN apt update \
    && apt upgrade -y \
    # install packages for development
    && apt -y install --no-install-recommends \
           vim \
           openssh-server \
           net-tools \
    # Delete cache to minimize the size of this image
    && rm -rf /var/lib/apt/lists/* \
    && apt -y clean \
    # for updating python module
    && pip install --upgrade pip

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

RUN { \
    echo "alias g='git'"; \
    echo "alias ll='ls $LS_OPTIONS -l'"; \
    echo "alias l='ls $LS_OPTIONS -lA'"; \
} >> ~/.bashrc
RUN . ~/.bashrc

WORKDIR /root/lab
