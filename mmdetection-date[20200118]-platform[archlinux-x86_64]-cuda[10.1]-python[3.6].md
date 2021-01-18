# open-mmlab env configure

Passed on : `20200118`  
Tester : `VisualDust | Miya@Akasaki.space`  
Using : `anaconda | pip`  
Platform : `Arch Linux` info below :  
screenfetch :

`````none
                   -`
                  .o+`                 visualdust@VDLaptop
                 `ooo/                 OS: Arch Linux
                `+oooo:                Kernel: x86_64 Linux 5.10.7-arch1-1
               `+oooooo:               Uptime: 7h 13m
               -+oooooo+:              Packages: 1635
             `/:-:++oooo+:             Shell: fish 3.1.2
            `/++++/+++++++:            Resolution: 2160x1440
           `/++++++++++++++:           DE: KDE 5.78.0 / Plasma 5.20.5
          `/+++ooooooooooooo/`         WM: KWin
         ./ooosssso++osssssso+`        GTK Theme: Breeze [GTK2/3]
        .oossssso-````/ossssss+`       Icon Theme: Tela-dark
       -osssssso.      :ssssssso.      Disk: 93G / 212G (44%)
      :osssssss/        osssso+++.     CPU: Intel Core i7-10510U @ 8x 4.9GHz [78.0°C]
     /ossssssss/        +ssssooo/-     GPU: GeForce MX250
   `/ossssso+/:-        -:/+osssso+-   RAM: 9758MiB / 15741MiB
  `+sso+:-`                 `.-/+oso:
 `++:.                           `-/+/
 .`                                 `/

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
conda create -n open-mmlab python=3.6 -y
conda activate open-mmlab
```

查看 nvcc 信息

```bash
nvcc -V
```

安装合适版本的 pytorch。请注意，其中的 10.1 是测试环境时对应的 cuda 版本

```bash
conda install pytorch==1.6.0 torchvision==0.7.0 cudatoolkit=10.1 -c pytorch
```

安装 mmcv-full

```bash
pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu101/torch1.6.0/index.html
```

克隆 mmdetection

```bash
git clone https://github.com/open-mmlab/mmdetection.git
cd mmdetection
```

安装 mmdetection

```bash
pip install -r requirements/build.txt
pip install -v -e .  # or "python setup.py develop
```

---

> 验证安装

运行下列 python 代码查看是否报错。如果没有，则安装成功。

```python
from mmdet.apis import init_detector, inference_detector
config_file = 'configs/faster_rcnn/faster_rcnn_r50_fpn_1x_coco.py'
device = 'cuda:0'
# init a detector
model = init_detector(config_file, device=device)
# inference the demo image
inference_detector(model, 'demo/demo.jpg')
```

---

> 可能出现的错误

- mmdetection No module named 'mmcv.\_ext'  
   这可能是 mmcv 安装错误导致的。应该安装的包为 mmcv-full，而安装的包是 mmcv。如果遇到这样的错误，请卸载 mmcv 包并重新安装 mmcv-full 包。具体安装方法参照上文中出现的命令行，或参考[官方说明](https://github.com/open-mmlab/mmcv)配置其他版本的 mmcv。相关说明：
  The rule for installing the latest mmcv-full is as follows:
  pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/{cu_version}/{torch_version}/index.html
  Please replace {cu_version} and {torch_version} in the url to your desired one. For example, to install the latest mmcv-full with CUDA 11 and PyTorch 1.7.0, use the following command:
  pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu110/torch1.7.0/index.html
  For more details, please refer the the following tables and delete =={mmcv_version}.

---

> 附录 : pip list after configuration

```none
addict             2.4.0
attrs              20.3.0
certifi            2020.12.5
cycler             0.10.0
Cython             0.29.21
importlib-metadata 3.4.0
iniconfig          1.1.1
kiwisolver         1.3.1
matplotlib         3.3.3
mmcv-full          1.2.5
mmdet              2.8.0
mmpycocotools      12.0.3
numpy              1.19.5
olefile            0.46
opencv-python      4.5.1.48
packaging          20.8
Pillow             8.1.0
pip                20.3.3
pluggy             0.13.1
py                 1.10.0
pyparsing          2.4.7
pytest             6.2.1
pytest-runner      5.2
python-dateutil    2.8.1
PyYAML             5.3.1
setuptools         49.6.0.post20210108
six                1.15.0
terminaltables     3.1.0
toml               0.10.2
torch              1.6.0
torchvision        0.7.0
typing-extensions  3.7.4.3
wheel              0.36.2
yapf               0.30.0
zipp               3.4.0
```
