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
conda create -n solo python=3.7
conda activate solo
```

查看 nvcc 信息

```bash
nvcc -V
```

安装合适版本的 pytorch。请注意，其中的 10.1 是测试环境时对应的 cuda 版本
由于作者开发此模型版本较老，请使用pytorch<=1.4.0以下的版本

```bash
conda install pytorch==1.4.0 torchvision==0.5.0 cudatoolkit=10.1 -c pytorch
```

安装 Cython

```bash
conda install cython
```

安装pycocotools
```bash
pip install "git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI"
```

克隆 SOLO

```bash
git clone https://github.com/WXinlong/SOLO.git
cd SOLO
```

安装 SOLO

```bash
python setup.py build develop
```

---

> 验证安装

运行下列 python 代码查看是否报错。如果没有，则安装成功。

从配置文件中选出一个网络，下载对应的模型，执行如下指令。
```bash
python demo/inference_demo.py
```

---

> 可能出现的错误

- 导入mmdet相关的包的时候提示ImportError: libtorch_cpu.so: cannot open shared object file: No such file or directory

这个意思是指找不到编译出来的so文件，因为这个环境配置是在torch的基础上进行开发，所以在build的时候会把torch进行编译，这里可能是因为编译过程中出现了问题导致没有生成.so文件导致导入失败。因为C++编译在linux上会生成.so文件。**解决方案：删除SOLO目录下的build文件夹重新进行编译。**
---

> 附录 : pip list after configuration

```none
Package                Version       
---------------------- ------------------- 
addict                 2.4.0
albumentations         0.5.2
certifi                2020.12.5
chardet                4.0.0
cycler                 0.10.0
Cython                 0.29.21
decorator              4.4.2
idna                   2.10
imagecorruptions       1.1.2
imageio                2.9.0
imgaug                 0.4.0
intel-openmp           2021.1.2
kiwisolver             1.3.1
matplotlib             3.3.3
mkl                    2021.1.1
mmcv                   0.2.16
mmdet                  1.0.0+b00170f      
networkx               2.5
numpy                  1.19.5
opencv-python          4.5.1.48
opencv-python-headless 4.5.1.48
Pillow                 6.2.2
pip                    20.3.3
pycocotools            2.0
pyparsing              2.4.7
python-dateutil        2.8.1
PyWavelets             1.1.1
PyYAML                 5.4
requests               2.25.1
scikit-image           0.18.1
scipy                  1.6.0
setuptools             51.3.3.post20210118
Shapely                1.7.1
six                    1.15.0
tbb                    2021.1.1
terminaltables         3.1.0
tifffile               2021.1.14
torch                  1.4.0
torchvision            0.5.0
urllib3                1.26.2
wheel                  0.36.2


```
