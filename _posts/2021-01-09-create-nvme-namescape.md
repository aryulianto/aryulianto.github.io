---
layout: post
title: Create NVMe Namescape - NVMe Namescape Calculator
categories:
  - Life
  - Linux
  - Trip
tags:
  - "2019"
  - Conference
  - Field
  - openSUSE
  - Report
---

1. Install package
```
apt -y install nvme-cli
```
2. List of NVMe devices
```
fdisk -l
nvme list
```
sample output:
```
Node             SN                   Model                Namespace Usage                    Format           FW Rev
---------------- -------------------- -------------------- --------- ------------------------ ---------------- --------
/dev/nvme0n1     BTLJ034400811P0FGN   INTEL SSDPE2KX010T8     1      1.00  TB /   1.00  TB    512   B +  0 B   VDV10170
/dev/nvme1n1     BTLJ0344008L1P0FGN   INTEL SSDPE2KX010T8     1      1.00  TB /   1.00  TB    512   B +  0 B   VDV10170
```

3. Check Namespace ID
```
nvme id-ns /dev/nvme{0..1} -n1
```
```nvme id-ns /dev/nvme0 -n1
NVME Identify Namespace 1:
nsze    : 0x74706db0
ncap    : 0x74706db0
nuse    : 0x74706db0
nsfeat  : 0
nlbaf   : 1
flbas   : 0
mc      : 0
dpc     : 0
dps     : 0
nmic    : 0
rescap  : 0
fpi     : 0
dlfeat  : 0
nawun   : 0
nawupf  : 0
nacwu   : 0
nabsn   : 0
nabo    : 0
nabspf  : 0
noiob   : 0
nvmcap  : 1000204886016
nsattr  : 0
nvmsetid: 0
anagrpid: 0
endgid  : 0
nguid   : 01000000010000005cd2e40d4dd55251
eui64   : 5cd2e40d4dd50100
lbaf  0 : ms:0   lbads:9  rp:0x2 (in use)
lbaf  1 : ms:0   lbads:12 rp:0
```

4. 