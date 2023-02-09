# PVE掉N卡驱动

## 报错信息

今天突然发现我的 PVE 的 N 卡驱动掉了，下面是一些检查结果：

```shell
# nvidia-smi
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. 
Make sure that the latest NVIDIA driver is installed and running.

# nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130

# ls /usr/src | grep nvidia
nvidia-current-520.61.05
```

## 解决过程

然后按照找了一篇博客，说要执行如下两条命令：

```shell
sudo apt-get install dkms
sudo dkms install -m nvidia -v 520.61.05
```

首先我遇到的问题就是说没有 **nvidia-520.61.05** 这个文件夹，通过上述执行发现我的是 **nvidia-current-520.61.05** 这个文件夹，如果前边的 **nvidia-** 是自动拼接的那就应该是执行如下命令：

```shell
sudo dkms install -m nvidia -v current-520.61.05
```

果然，这样就不在报这个上边的错误了。

然后有继续报错了，是说 **pve-headers-5.15.74-1-pve_5.15.74** 这个包找不到，我记得当初我安装驱动的时候就是找不到 **pve-headers-xxx** 这样的内核头文件的包，折腾了很久，最后在一个源里扒到了 deb 包，然后下载下来手动安装的，下边是下载链接：

```shell
http://download.proxmox.com/debian/pve/dists/bullseye/pve-no-subscription/binary-amd64/
```

下载对应的 deb 包，上传到 PVE 然后手动安装一下就好了。

```shell
# nvidia-smi
Thu Jan  5 22:16:47 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 520.61.05    Driver Version: 520.61.05    CUDA Version: 11.8     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:07:00.0 Off |                  N/A |
| 40%   33C    P0    14W / 125W |      0MiB /  6144MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

## 分析

我我觉得可能是因为不小心更新了内核，然后又找不到对应的  **pve-headers-xxx** 包，所以驱动掉了，只要手动安装上 **pve-headers-xxx** 就行了。