Title: 2025 TRex Testing (Finland, 20-28 Aug)
Date: 2025-08-28
Summary: TRex Testing: A Quick Start on Ubuntu 24.04 - 25.04


## Overview
This guide provides a quick start for installing and testing TRex (Cisco's Traffic Generator) on Ubuntu 24.04 (Noble Numbat) and 25.04 (Plucky Puffin).

### Prerequisites
Before installing TRex, ensure your system meets the following requirements:

- **Operating System:** Ubuntu 24.04 LTS or Ubuntu 25.04
- **Python:** Python 3.9 (TRex v3.06 is tied to this version)
- **CPU:** Multi-core processor with NICs bound to the cores you are using
- **Network:** Tested with Mellanox ConnectX-5 or ConnectX-7
- **DOCA-OFED:** Must match your OS distribution — https://developer.nvidia.com/networking/doca

## Installation

### Installing TRex with Python 3.9
For these tests, I used TRex v3.06 (as of July 2025). This version requires an
older Python runtime (3.8-3.9) than what ships with modern Linux distributions
such as Ubuntu 25.04.

Download the Python 3.9 source tarball:

`wget https://www.python.org/ftp/python/3.9.13/Python-3.9.13.tgz`

Extract it, run `configure`, and install.

To always use Python 3.9, create a virtual environment (one-time setup). After that, activate this venv every time you work with TRex.

Run this from `/root` or another persistent location:

```bash
python3.9 -m venv venv
```

### Log in and activate the Python 3.9 venv

```bash
cd /root
source ./venv/bin/activate
# To verify:
type python3.9; #python3.9 is hashed (/root/venv/bin/python3.9)
```

## Installing DOCA-OFED
For setups using Mellanox ConnectX NICs, DOCA-OFED is required to ensure proper driver and DPDK support. Configuration details are provided at the end of this document.

```bash
wget https://www.mellanox.com/downloads/DOCA/DOCA_v3.0.0/host/doca-host_3.0.0-058000-25.04-ubuntu2504_amd64.deb
dpkg -i doca-host_3.0.0-058000-25.04-ubuntu2504_amd64.deb
apt update
apt -y install doca-ofed
dkms autoinstall
mst start
mst status -v
```


### Downloading TRex

```bash
wget --no-check-certificate --no-cache https://trex-tgn.cisco.com/trex/release/latest
tar xvf latest
mv v3.06 trex-v3.06/
cd trex-v3.06/
git init
git add .
git commit
rm  so/x86_64/libstdc++.so.6
cd ../
mv trex-v3.06 /var/tmp
sysctl -w vm.nr_hugepages=2048  # add this to /etc/sysctl.d to make it permanent.
```
At the time of writing, `latest` pointed to v3.06.

## Starting TRex Server
TRex is installed under `/var/tmp/trex-v3.06` on the sunset host.

```bash
sysctl -w vm.nr_hugepages=2048
source /root/venv/bin/activate
cd /var/tmp/trex-v3.06
./t-rex-64 -i --no-scapy --cfg /etc/trex_cfg.yaml -c 8
```

Start the TRex server once and leave it running in the foreground. I prefer to run it inside a `screen` session so you can reattach later with `screen -x` to see the output. The server must be started within the Python 3.9 venv.

I recommend starting `screen bash` from the activated virtual environment, then launching the TRex server inside that screen session.

It will take a few seconds to start and will remain running in the foreground.

### Running a TRex Test Script

From a second terminal (also inside the Python 3.9 venv), run one of the test scripts. For example:

```bash
cd /root/ietf-123-pcpu/tests-trex
./u1.py
./u1.py --src-ip 192.0.1.253 --dst-ip 192.0.2.253 --pps 1M --frame-size 1518 --flows 2 --duration 10 --flows-end 2 --runs 2
```

`u1.py` is a simple UDP sender/receiver that measures throughput and packet loss. It stores results in JSON, which are later processed with Pandas and plotted using Matplotlib.

## Appendix A: Mellanox DOCA-OFED Diagnostics
This section is only needed if you are troubleshooting your Mellanox NIC setup. If TRex starts and traffic flows, you can skip this.

After installing DOCA-OFED, run `mst start` once. To verify that your NICs are detected, check the output of `mst status -v`:
```
 mst status -v
MST modules:
------------
    MST PCI module is not loaded
    MST PCI configuration module loaded
    -E- Unknown argument "--v"
root@sunset:~/ietf-123-pcpu/tests-trex# mst status -v
MST modules:
------------
    MST PCI module is not loaded
    MST PCI configuration module loaded
PCI devices:
------------
DEVICE_TYPE             MST                           PCI       RDMA                                               NET                                     NUMA
ConnectX5(rev:0)        /dev/mst/mt4121_pciconf0.1    01:00.1   mlx5_1          net-redwest                             0

ConnectX5(rev:0)        /dev/mst/mt4121_pciconf0      01:00.0   mlx5_0          net-redeast                             0
```
