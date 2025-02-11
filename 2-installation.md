## 2. Installation
### 2.1 Prerequisite

Vitis HLS should be installed. In Windows platform, please execute the hls_tools in a WSL environment.

Please install below package:
```bash
sudo apt update
sudo apt install python3 gcc-multilib g++-multilib libclang-dev clang

pip3 install libclang
pip3 install colorama
```

### 2.2 Extract the Package
```bash
#Below command will extract to d:\hls_tools directory in WSL
#You could choose any other dest path
mkdir -p /mnt/d/hls_tools
tar -xzvf hls_tools.tar.gz -C /mnt/d/hls_tools
```