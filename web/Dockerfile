#FROM tensorflow/tensorflow:1.8.0-gpu-py3
FROM tensorflow/tensorflow:1.8.0-py3
WORKDIR "/"

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONIOENCODING=utf-8

# install packages
## Preperation
RUN apt-get update -y --fix-missing
RUN apt-get install -y apt-utils
RUN apt-get install -y python3-pip

## for ffmpeg
RUN add-apt-repository ppa:jonathonf/ffmpeg-3
RUN apt update -y
RUN apt install -y ffmpeg libav-tools x264 x265

## for vmd-lifting
RUN apt-get install -y python3-pip
RUN apt-get install -y python3-tk
RUN apt-get install -y python3-pyqt5
RUN apt-get install -y cmake

## others
RUN apt-get install -y wget
RUN apt-get install -y vim

# install python packages
## Preparation
RUN pip3 install --upgrade pip

## for ffmpeg
RUN pip3 install ffmpeg-python

## for vmd-lifting
#RUN pip3 install tensorflow-gpu
#RUN pip3 install tensorflow
RUN pip3 install scikit-image
RUN pip3 install opencv-python==3.4.2.17

## for vc
RUN pip3 install librosa
RUN pip3 install pyworld
## ↑エラーが出るがなぜかsuccessfilly installed になる(要調査)

## for app
RUN pip3 install flask
RUN pip3 install flask_sqlalchemy
RUN pip3 install moviepy

## for compose
RUN pip3 install gunicorn

# ↓---for remote build---↓
RUN curl -L -O https://github.com/one-color-low/ReBone_v2/archive/master.zip
RUN unzip master.zip && rm master.zip

RUN mv ReBone_v2-master ReBone_v2

RUN mkdir unko
RUN /ReBone_v2/rebone_VC/setup.sh
RUN unzip pretrain_data.zip && rm pretrain_data.zip

# ※↓最初からpretrain_dataディレクトリをgitから削除しておくことでやらなくて済む
RUN rm /ReBone_v2/rebone_VC/pretrain_data/cache/dummy
RUN rm -r /ReBone_v2/rebone_VC/pretrain_data

RUN mv /pretrain_data /ReBone_v2/rebone_VC
# ↑---for remote build---↑


# ↓---for local build---↓
#RUN mkdir ReBone_v2
#COPY ./ /ReBone_v2
# ↑--for local build---↑

## setups
RUN chmod +x /ReBone_v2/rebone_vmdl/setup.sh
RUN /ReBone_v2/rebone_vmdl/setup.sh
RUN mv /data /ReBone_v2/rebone_vmdl

WORKDIR /ReBone_v2

RUN touch gunicorn_conf.py

RUN rm gunicorn_conf.py

COPY gunicorn_conf.py ./

CMD ["gunicorn", "app:app", "-b", ":5000", "-c", "gunicorn_conf.py"]

#CMD [ "bash" ]