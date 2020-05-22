FROM python:3.8.1

RUN apt update \
    && apt upgrade -y \
    # install packages for development
    && apt -y install --no-install-recommends \
           vim openssh-server \
    # imageのサイズを小さくするためにキャッシュ削除
    && rm -rf /var/lib/apt/lists/* \
    && apt -y clean \
    # for ssh login: The opened portNo is 222
    && mkdir /var/run/sshd \
    && echo 'root:password' | chpasswd \
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && sed -i '/pam_loginuid\.so/s/required/optional/' /etc/pam.d/sshd \
    && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#Port 22/Port 222/' /etc/ssh/sshd_config \
    # for updating python module
    && pip install --upgrade pip

WORKDIR /home/lab
# RUN git clone https://github.com/siruku6/fx_alarm_py.git \
#     && cd fx_alarm_py \
#     && pip install -r requirements.txt \
RUN sed -i '$a alias g='\''git'\' ~/.bashrc \
    && sed -i '$a alias ll='\''ls $LS_OPTIONS -l'\' ~/.bashrc \
    && sed -i '$a alias l='\''ls $LS_OPTIONS -lA'\' ~/.bashrc \
    && . ~/.bashrc

EXPOSE 222
