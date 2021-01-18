# open-mmlab env configure

Passed on : `20200118`  
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
| NVIDIA-SMI 450.80.02    Driver Version: 450.80.02    CUDA Version: 11.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce RTX 208...  Off  | 00000000:5E:00.0 Off |                  N/A |
| 23%   22C    P8    20W / 250W |      6MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  GeForce RTX 208...  Off  | 00000000:D8:00.0 Off |                  N/A |
| 28%   23C    P8    19W / 250W |      6MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+    
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1556      G   /usr/lib/xorg/Xorg                  4MiB |
|    1   N/A  N/A      1556      G   /usr/lib/xorg/Xorg                  4MiB |
+-----------------------------------------------------------------------------+


```
---

> Shell script

创建并激活环境

```bash
conda create -n open-mmlab python=3.7
conda activate open-mmlab
```

查看 nvcc 信息

```bash
nvcc -V
```

安装合适版本的 pytorch 和 mmcv-full。请注意，其中的 10.1 是测试环境时对应的 cuda 版本

```bash
pip install mmcv-full==latest+torch1.6.0+cu101 -f https://download.openmmlab.com/mmcv/dist/index.html
```

克隆 mmdetection

```bash
git clone https://github.com/open-mmlab/mmsegmentation.git
cd mmsegmentation
```

安装 mmdetection

```bash
pip install -r requirements/runtime.txt
pip install -e .  # or "python setup.py develop
```

---

> 验证安装

运行下列 python 代码查看是否报错。如果没有，则安装成功。

```python
from mmdet.apis import init_detector, inference_detector
config_file = 'configs/pspnet/pspnet_r50-d8_512x1024_40k_cityscapes.py'
checkpoint_file = 'checkpoints/pspnet_r50-d8_512x1024_40k_cityscapes_20200605_003338-2966598c.pth'
device = 'cuda:0'

# build the model from a config file and a checkpoint file
model = init_segmentor(config_file, checkpoint_file, device=device)

# test a single image and show the results
img = 'demo/demo.png'  # or img = mmcv.imread(img), which will only load it once
result = inference_segmentor(model, img)
# visualize the results in a new window
model.show_result(img, result, show=True)
# or save the visualization results to image files
model.show_result(img, result, out_file='result.png')

```

---

> 可能出现的错误

```none
Installing collected packages: mmsegmentation
  Running setup.py develop for mmsegmentation
    ERROR: Command errored out with exit status 1:
     command: /home/ray/anaconda3/envs/open-mmlab/bin/python -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/home/ray/workspace/PommesPeter/mmsegmentation/setup.py'"'"'; __file__='"'"'/home/ray/workspace/PommesPeter/mmsegmentation/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' develop --no-deps 
     cwd: /home/ray/workspace/PommesPeter/mmsegmentation/           
    Complete output (14 lines):                             
running develop                                         
running egg_info                         
creating mmsegmentation.egg-info 
writing mmsegmentation.egg-info/PKG-INFO
writing dependency_links to mmsegmentation.egg-info/dependency_links.txt
writing requirements to mmsegmentation.egg-info/requires.txt
writing top-level names to mmsegmentation.egg-info/top_level.txt
writing manifest file 'mmsegmentation.egg-info/SOURCES.txt'
reading manifest file 'mmsegmentation.egg-info/SOURCES.txt'         
writing manifest file 'mmsegmentation.egg-info/SOURCES.txt'
running build_ext Creating /home/ray/anaconda3/envs/open-mmlab/lib/python3.7/site-packages/mmsegmentation.egg-link (link to .)                                                           
Adding mmsegmentation 0.10.0 to easy-install.pth file
error: [Errno 13] Permission denied: '/home/ray/anaconda3/envs/open-mmlab/lib/python3.7/site-packages/easy-install.pth'                                                
----------------------------------------                                                             
ERROR: Command errored out with exit status 1: /home/ray/anaconda3/envs/open-mmlab/bin/python -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/home/ray/workspace/PommesPeter/mmsegmentation/setup.py'"'"'; __file__='"'"'/home/ray/workspace/PommesPeter/mmsegmentation/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"' 
```

需要重新运行一下安装指令，v并且加入--user后缀

```bash
pip install -e . --user
```

---

> 附录 : conda list after configuration

```none
Package                       Version            
----------------------------- ------------------- 
# Name                    Version                   
_libgcc_mutex             0.1                      
addict                    2.3.0                          
alabaster                 0.7.12                         
appdirs                   1.4.4                           
babel                     2.9.0                         
backcall                  0.2.0                          
ca-certificates           2020.10.14                    
certifi                   2020.6.20                      
chardet                   4.0.0                          
cityscapesscripts         2.1.7                          
coloredlogs               14.0                           
commonmark                0.9.1                          
cycler                    0.10.0                         
cython                    0.29.21                        
decorator                 4.4.2                          
docutils                  0.16                           
future                    0.18.2                         
humanfriendly             8.2                            
idna                      2.10                           
imagecorruptions          1.1.0                          
imageio                   2.9.0                          
imagesize                 1.2.0                          
importlib-metadata        3.4.0                          
ipython                   7.19.0                         
ipython-genutils          0.2.0                          
jedi                      0.18.0                         
jinja2                    2.11.2                         
kiwisolver                1.3.1                          
ld_impl_linux-64          2.33.1              
libedit                   3.1.20191231         
libffi                    3.3                  
libgcc-ng                 9.1.0         
libstdcxx-ng              9.1.0                
markdown                  3.3.3                          
markupsafe                1.1.1                          
matplotlib                3.3.2                          
mmcv-full                 1.2.5                          
mmdet                     2.5.0                         
mmpycocotools             12.0.3                         
ncurses                   6.2         
networkx                  2.5                            
numpy                     1.19.3                         
opencv-python             4.4.0.44                       
openssl                   1.1.1h    
packaging                 20.8                           
parso                     0.8.1                          
pexpect                   4.8.0                          
pickleshare               0.7.5                          
pillow                    8.0.1                          
pip                       20.2.4            
prompt-toolkit            3.0.10                         
ptyprocess                0.7.0                          
pygments                  2.7.4                          
pyparsing                 2.4.7                          
pyquaternion              0.9.9                          
python                    3.7.9              
python-dateutil           2.8.1                          
pytz                      2020.5                         
pywavelets                1.1.1                          
pyyaml                    5.3.1                          
readline                  8.0             
recommonmark              0.7.1                          
requests                  2.25.1                         
scikit-image              0.17.2                         
scipy                     1.5.3                          
setuptools                50.3.0        
six                       1.15.0                         
snowballstemmer           2.0.0                          
sphinx                    3.4.3                          
sphinx-markdown-tables    0.0.15                         
sphinx-rtd-theme          0.5.1                          
sphinxcontrib-applehelp   1.0.2                          
sphinxcontrib-devhelp     1.0.2                          
sphinxcontrib-htmlhelp    1.0.3                          
sphinxcontrib-jsmath      1.0.1                          
sphinxcontrib-qthelp      1.0.3                          
sphinxcontrib-serializinghtml 1.1.4                          
sqlite                    3.33.0            
tifffile                  2020.10.1                      
tk                        8.6.10           
torch                     1.6.0+cu101                    
torchvision               0.7.0+cu101                    
tqdm                      4.51.0                         
traitlets                 5.0.5                          
typing                    3.7.4.3                        
typing-extensions         3.7.4.3                        
urllib3                   1.26.2                         
wcwidth                   0.2.5                          
wheel                     0.35.1                
xz                        5.2.5                
yapf                      0.30.0                         
zipp                      3.4.0                          
zlib                      1.2.11               
```
 
