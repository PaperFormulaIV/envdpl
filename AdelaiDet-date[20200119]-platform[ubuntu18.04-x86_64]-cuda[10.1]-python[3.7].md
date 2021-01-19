# open-mmlab env configure

Passed on : `20200119`  
Tester : `PommesPeter | me@pommespeter.com`  
Using : `anaconda | pip`  
Platform : `Ubuntu18.04` info below :  
screenfetch :

`````none
                          ./+o+-       ray@ProfessorRay
                  yyyyy- -yyyyyy+      OS: Ubuntu 18.04 bionic
               ://+//////-yyyyyyo      Kernel: x86_64 Linux 5.4.0-60-generic
           .++ .:/++++++/-.+sss/`      Uptime: 1d 4h 4m
         .:++o:  /++++++++/:--:/-      Packages: 1768
        o:+o+:++.`..```.-/oo+++++/     Shell: bash
       .:+o:+o/.          `+sssoo+/    CPU: Intel Xeon Silver 4210 @ 40x 3.2GHz [39.0°C]
  .++/+:+oo+o:`             /sssooo.   GPU: GeForce RTX 2080 Ti, GeForce RTX 2080 Ti
 /+++//+:`oo+o               /::--:.   RAM: 1240MiB / 64052MiB
 \+/+o+++`o++o               ++////.  
  .++.o+++oo+:`             /dddhhh.  
       .+.o+oo:.          `oddhhhh+   
        \+.++o+o``-````.:ohdhhhhh+    
         `:o+++ `ohhhhhhhhyo++os:     
           .o:`.syhhhhhhh/.oo++o`     
               /osyyyyyyo++ooo+++/    
                   ````` +oo+++o\:    
                          `oo++.    

`````

nvcc -V :

```none
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243
```

nvidia-smi :

```none
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.32.03    Driver Version: 460.32.03    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce MX250       Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   43C    P8    N/A /  N/A |      4MiB /  2002MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A       775      G   /usr/lib/Xorg                       4MiB |
+-----------------------------------------------------------------------------+

```

---

> Shell script

创建并激活环境

```bash
conda create -n adelaiDet python=3.6 -y
conda activate adelaiDet
```

查看 nvcc 信息

```bash
nvcc -V
```

安装合适版本的 pytorch。请注意，其中的 10.1 是测试环境时对应的 cuda 版本

```bash
conda install pytorch==1.6.0 torchvision==0.7.0 cudatoolkit=10.1 -c pytorch
```

安装 Ddetectron2

```bash
pip install 'git+https://github.com/facebookresearch/detectron2.git'
```

克隆 AdelaiDet

```bash
git clone https://github.com/aim-uofa/AdelaiDet.git
cd AdelaiDet
```

安装 AdelaiDet

```bash
python setup.py build develop
```

---

> 验证安装

运行下列 python 代码查看是否报错。如果没有，则安装成功。

从配置文件中选出一个网络，下载对应的模型，执行如下指令。
```bash
python demo/demo.py \
    --config-file configs/FCOS-Detection/R_50_1x.yaml \
    --input input1.jpg input2.jpg \
    --opts MODEL.WEIGHTS fcos_R_50_1x.pth
```

---

> 可能出现的错误

- 暂无，如果有人配置的时候遇到了请更新此文件

---

> 附录 : pip list after configuration

```none
absl-py                0.11.0
AdelaiDet              0.2.0               
backcall               0.2.0
cachetools             4.2.0
certifi                2020.12.5
chardet                4.0.0
cloudpickle            1.6.0
cycler                 0.10.0
Cython                 0.29.21
decorator              4.4.2
detectron2             0.3
future                 0.18.2
fvcore                 0.1.2.post20210115
google-auth            1.24.0
google-auth-oauthlib   0.4.2
grpcio                 1.34.1
idna                   2.10
importlib-metadata     3.4.0
iopath                 0.1.2
ipython                7.19.0
ipython-genutils       0.2.0
jedi                   0.18.0
kiwisolver             1.3.1
Markdown               3.3.3
matplotlib             3.3.3
mkl-fft                1.2.0
mkl-random             1.1.1
mkl-service            2.3.0
numpy                  1.19.2
oauthlib               3.1.0
olefile                0.46
opencv-python          4.5.1.48
parso                  0.8.1
pexpect                4.8.0
pickleshare            0.7.5
Pillow                 8.1.0
pip                    20.3.3
Polygon3               3.0.8
portalocker            2.0.0
prompt-toolkit         3.0.10
protobuf               3.14.0
ptyprocess             0.7.0
pyasn1                 0.4.8
pyasn1-modules         0.2.8
pycocotools            2.0.2
pydot                  1.4.1
Pygments               2.7.4
pyparsing              2.4.7
python-dateutil        2.8.1
python-Levenshtein     0.12.1
PyYAML                 5.3.1
requests               2.25.1
requests-oauthlib      1.3.0
rsa                    4.7
setuptools             51.1.2.post20210112
Shapely                1.7.1
six                    1.15.0
tabulate               0.8.7
tensorboard            2.4.1
tensorboard-plugin-wit 1.7.0
termcolor              1.1.0
torch                  1.6.0
torchvision            0.7.0
tqdm                   4.56.0
traitlets              5.0.5
typing-extensions      3.7.4.3
urllib3                1.26.2
wcwidth                0.2.5
Werkzeug               1.0.1
wheel                  0.36.2
yacs                   0.1.8
zipp                   3.4.0

```
