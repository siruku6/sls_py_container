FROM python:3.8.1

RUN apt-get update \
    && apt-get upgrade -y \
    # imageのサイズを小さくするためにキャッシュ削除
    && apt-get clean \
    && apt-get -y install vim \
    && rm -rf /var/lib/apt/lists/* \
    # pipのアップデート
    && pip install --upgrade pip

WORKDIR /home/lab
# RUN git clone https://github.com/siruku6/fx_alarm_py.git \
#     && cd fx_alarm_py \
#     && pip install -r requirements.txt \
RUN sed -i '$a alias g='\''git'\' ~/.bashrc \
    && sed -i '$a alias ll='\''ls $LS_OPTIONS -l'\' ~/.bashrc \
    && sed -i '$a alias l='\''ls $LS_OPTIONS -lA'\' ~/.bashrc \
    && . ~/.bashrc