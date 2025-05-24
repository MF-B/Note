---
dg-publish: false
---
2025-05-06
Status: #idea
Tags: [[Readme]]

# EulixOS Stage 4
## 软件列表
![[微信图片_20250516142239.png]]
dnf install wget

- python
	1. dnf install python
- bitsandbytes
	1. dnf install openblas
	2. pip
- transformers
	1. pip
- ftfy
	1. pip
- regex
	1. pip
- einops
	1. pip
- fvcore
	1. dnf install libjpeg-devel zlib-devel libpng-devel
	2. pip 
- iopath
	1. pip
- matplotlib
	1. pip
- numpy
	1. pip
- moviepy
	1. pip
- types-regex
	1. pip
- neo4j
	1. pip
- hnswlib
	1. pip
- xxhash
	1. pip
- nano-vectordb
	1. pip
- tenacity
	1. pip
- langchain
	1. dnf install libffi-devel
	2. pip
- langchain-core
	1. pip
- langchain-text-splitters
	1. pip
- langchain-community
	1. pip
- langchain-openai
	1. pip
- volcengine
	1. pip
- torch
```bash
dnf install musl

git clone https://github.com/pytorch/pytorch
cd pytorch
# if you are updating an existing checkout
git submodule sync
git submodule update --init --recursive

export USE_CUDA=0 # RISC-V架构服务器无法使用CUDA
export USE_DISTRIBUTED=0 # 不支持分布式
export USE_MKLDNN=0 # 并非英特尔处理器，故不支持MKL
export MAX_JOBS=5 # 编译进程数，根据自己实际需求进行更改
python3 setup.py develop --cmake

```
- torch-version
- torch-audio
- eva-decord
```bash
dnf install ffmpeg ffmpeg-devel
# git clone --recursive https://github.com/zhanwenchen/decord.git
git clone --recursive https://github.com/MF-B/eva-decord.git
cd eva-decord
mkdir build
cd build
cmake .. -DUSE_CUDA=0 -DCMAKE_BUILD_TYPE=Release
make
cd ../python && python3 setup.py install
```
- ctranslate2
```bash
git clone --recursive https://github.com/OpenNMT/CTranslate2.git
cd CTranslate2
mkdir build && cd build
cmake .. -DOPENMP_RUNTIME=COMP -DENABLE_CPU_DISPATCH=OFF
make -j4
sudo make install
sudo ldconfig
cd ../python
pip install -r install_requirements.txt
python setup.py bdist_wheel
pip install dist/*.whl
```

- milvus-lite
	1. pip install -U pymilvus
- accelerate
- timm
- pymilvus
- langchain-milvus
## [PyTorch构建](https://github.com/pytorch/pytorch#from-source)
1. python
	- version >=3.9
	- dnf install python3 python3-devel
2. 开发工具
	- dnf group install "Development Tools"
	- rust安装
3. 虚拟环境
	- 使用pyvenv
	- dnf install -y python3-virtualenv
	- update-alternatives --install /usr/bin/python python /usr/bin/python3 10
	- python -m venv .venv && source .venv/bin/activate
4. 源码编译
```bash
git clone https://github.com/pytorch/pytorch
cd pytorch
# if you are updating an existing checkout
git submodule sync
git submodule update --init --recursive
```



___
# References