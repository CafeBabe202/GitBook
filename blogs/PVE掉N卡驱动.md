# ð PVEæNå¡é©±å¨

## æ¥éä¿¡æ¯

ä»å¤©çªç¶åç°æç PVE ç N å¡é©±å¨æäºï¼ä¸é¢æ¯ä¸äºæ£æ¥ç»æï¼

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

## è§£å³è¿ç¨

ç¶åæç§æ¥éä¿¡æ¯æ¾äºä¸ç¯åå®¢ï¼è¯´è¦æ§è¡å¦ä¸ä¸¤æ¡å½ä»¤ï¼

```shell
sudo apt-get install dkms
sudo dkms install -m nvidia -v 520.61.05
```

é¦åæéå°çé®é¢å°±æ¯è¯´æ²¡æ **nvidia-520.61.05** è¿ä¸ªæä»¶å¤¹ï¼éè¿ä¸è¿°æ§è¡åç°æçæ¯ **nvidia-current-520.61.05** è¿ä¸ªæä»¶å¤¹ï¼å¦æåè¾¹ç **nvidia-** æ¯èªå¨æ¼æ¥çé£å°±åºè¯¥æ¯æ§è¡å¦ä¸å½ä»¤ï¼

```shell
sudo dkms install -m nvidia -v current-520.61.05
```

æç¶ï¼è¿æ ·å°±ä¸å¨æ¥è¿ä¸ªä¸è¾¹çéè¯¯äºã

ç¶ååç»§ç»­æ¥éäºï¼æ¯è¯´ **pve-headers-5.15.74-1-pve\_5.15.74** è¿ä¸ªåæ¾ä¸å°ï¼æè®°å¾å½åæå®è£é©±å¨çæ¶åå°±æ¯æ¾ä¸å° **pve-headers-xxx** è¿æ ·çåæ ¸å¤´æä»¶çåï¼æè¾äºå¾ä¹ï¼æåå¨ä¸ä¸ªæºéæå°äº deb åï¼ç¶åä¸è½½ä¸æ¥æå¨å®è£çï¼ä¸è¾¹æ¯ä¸è½½é¾æ¥ï¼

```shell
http://download.proxmox.com/debian/pve/dists/bullseye/pve-no-subscription/binary-amd64/
```

ä¸è½½å¯¹åºç deb åï¼ä¸ä¼ å° PVE ç¶åæå¨å®è£ä¸ä¸å°±å¥½äºã

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

## åæ

æè§å¾å¯è½æ¯å ä¸ºä¸å°å¿æ´æ°äºåæ ¸ï¼ç¶ååæ¾ä¸å°å¯¹åºç **pve-headers-xxx** åï¼æä»¥é©±å¨æäºï¼åªè¦æå¨å®è£ä¸ **pve-headers-xxx** å°±è¡äºã
