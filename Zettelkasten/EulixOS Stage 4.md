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

1. python
	1. dnf
2. bitsandbytes
	1. dnf install openblas
	2. pip
3. transformers
	1. pip
4. ftfy
	1. pip
5. regex
	1. pip
6. einops
	1. pip
7. fvcore
	1. dnf install libjpeg-devel zlib-devel libpng-devel
	2. pip 
8. iopath
	1. pip
9. matplotlib
	1. pip
10. numpy
	1. pip
11. moviepy
	1. pip
12. types-regex
	1. pip
13. neo4j
	1. pip
14. hnswlib
	1. pip
15. xxhash
	1. pip
16. nano-vectordb
	1. pip
17. tenacity
	1. pip
18. langchain
	1. dnf install libffi-devel
	2. pip
19. langchain-core
	1. pip
20. langchain-text-splitters
	1. pip
21. langchain-community
	1. pip
22. langchain-openai
	1. pip
23. volcengine
	1. pi
24. torch
25. torch-version
26. torch-audio
27. eva-decord
	1. dnf install ffmpeg ffmpeg-devel
	2. git clone --recursive https://github.com/zhanwenchen/decord.git
	3. mkdir build & cd build
	4. cmake .. -DUSE_CUDA=0 -DCMAKE_BUILD_TYPE=Release
	5. make
	6. cd ../python && python3 setup.py install
28. ctranslate2
	1. git clone --recursive https://github.com/OpenNMT/CTranslate2.git
	2. cd CTranslate2
	3. mkdir build && cd build
	4. cmake .. -DOPENMP_RUNTIME=COMP -DENABLE_CPU_DISPATCH=OFF
	5. make -j4
	6. sudo make install
	7. sudo ldconfig
	8. cd ../python
	9. pip install -r install_requirements.txt
	10. python setup.py bdist_wheel
	11. pip install dist/*.whl
29. milvus-lite
	1. 
30. accelerate
31. timm
32. pymilvus
33. langchain-milvus
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