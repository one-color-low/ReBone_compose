FROM nvcr.io/nvidia/tensorflow:18.07-py3

WORKDIR /
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONIOENCODING=utf-8
ENV LANG=C.UTF-8

# install packages
## Preperation
RUN apt-get update -y
RUN apt-get install -y apt-utils
RUN apt-get install -y python3-pip
RUN apt-get install software-properties-common -y

## for ffmpeg
RUN add-apt-repository ppa:jonathonf/ffmpeg-3
RUN apt update -y
RUN apt install -y ffmpeg libav-tools x264 x265

## for pose_set_mod
RUN apt-get install -y curl unzip lv
RUN apt-get install -y libsm6 libgl1 libxrender1

# gpuにする場合は以下をコメントアウト
RUN pip uninstall tensorflow
RUN pip install tensorflow==1.8.0

## others
RUN apt-get install -y git
RUN apt-get install -y wget
RUN apt-get install -y vim

# install python packages
## Preparation
RUN pip3 install --upgrade pip

## for ffmpeg
RUN pip3 install ffmpeg-python

## for pose_est_mod
### FCRN-DepthPrediction-vmd
RUN pip3 install python-dateutil
RUN pip3 install pytz
RUN pip3 install pyparsing
RUN pip3 install six
RUN pip3 install matplotlib
RUN pip3 install opencv-python==3.4.2.17
RUN pip3 install imageio
### 3d-pose-baseline-multi
RUN pip3 install h5py
### VMD-3d-pose-baseline-multi
RUN pip3 install PyQt5
### tf-pose-estimation
RUN apt-get install -y swig
RUN pip3 install Cython
RUN pip3 install argparse
RUN pip3 install dill
RUN pip3 install fire
RUN pip3 install matplotlib
RUN pip3 install numba
RUN pip3 install psutil
RUN pip3 install pycocotools
RUN pip3 install requests
RUN pip3 install scikit-image
RUN pip3 install scipy
RUN pip3 install slidingwindow
RUN pip3 install tqdm
RUN pip3 install git+https://github.com/ppwwyyxx/tensorpack.git

## for vc
RUN pip3 install librosa
RUN pip3 install pyworld

## for app
RUN pip3 install flask
RUN pip3 install flask_sqlalchemy
RUN pip3 install moviepy


# setups

## ---for deploy---
### アプリ本体
RUN curl -L -O https://github.com/one-color-low/ReBone_v2/archive/master.zip
RUN unzip master.zip && rm master.zip
RUN mv ReBone_v2-master ReBone_v2

### PE用のモジュールを用意
RUN curl -sc /tmp/cookie "https://drive.google.com/uc?export=download&id=1K1eF4mb2D19G27fnvTVzJRT-yPIKJpXW" > /dev/null
RUN CODE="$(awk '/_warning_/ {print $NF}' /tmp/cookie)"
RUN curl -Lb /tmp/cookie "https://drive.google.com/uc?export=download&confirm=${CODE}&id=1K1eF4mb2D19G27fnvTVzJRT-yPIKJpXW" -o pose_est_mod.zip
RUN unzip pose_est_mod.zip && rm pose_est_mod.zip
RUN mv pose_est_mod rebone_v2/

### VC用の学習モデルを用意
RUN /ReBone_v2/rebone_VC/setup.sh
RUN unzip pretrain_data.zip && rm pretrain_data.zip
RUN mv pretrain_data rebone_v2/rebone_VC/

## ---for local build---
#RUN mkdir ReBone_v2
#COPY ./ /ReBone_v2

WORKDIR /ReBone_v2/pose_est_mod/tf_pose_estimation/tf_pose/pafprocess
RUN swig -python -c++ pafprocess.i
RUN python3 setup.py build_ext --inplace

WORKDIR /ReBone_v2

#CMD ["gunicorn", "app:app", "-b", ":5000", "-c", "gunicorn_conf.py"]

#CMD [ "bash" ]

CMD [ "python","app.py" ]