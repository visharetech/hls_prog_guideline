## 3. Installation
### 3.1 Prerequisite

Vitis HLS should be installed. <br/>
[https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html)

In the Windows platform, please execute the hls_tools in the WSL environment.<br/>

Python 3.12 should be used (which is the default python version shipped in Ubuntu 24.04). Please contact us to generate a new package if a different python environment is used.

Please install below package:
```console
sudo apt update

sudo apt install python3 python3-pip python3-venv gcc-multilib g++-multilib libclang-dev clang cmake build-essential

pip3 install libclang
pip3 install tabulate
pip3 install colorama
```


If it has an error on installing libclang and colorama, please type below command to activate python virtual environment and install relevant python package on it.
```console
sudo apt install python3-venv
python3 -m venv myenv
source myenv/bin/activate
pip3 install libclang
pip3 install tabulate
pip3 install colorama
```

### 3.2 Install RISC-V compiler toolchain
```console
wget -O riscv32-toolchain.tar.gz https://github.com/stnolting/riscv-gcc-prebuilt/releases/download/rv32i-4.0.0/riscv32-unknown-elf.gcc-12.1.0.tar.gz
sudo mkdir -p /opt/riscv
sudo tar -xzf riscv32-toolchain.tar.gz -C /opt/riscv/
rm -f riscv32-toolchain.tar.gz

#append the toolchain to the PATH variable in .bashrc
echo 'export PATH=$PATH:/opt/riscv/bin:/opt/asim' >> ~/.bashrc

source ~/.bashrc
```
