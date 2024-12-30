# EpicChain Development Environment - An In-Depth Guide

Welcome to the **EpicChain Development Environment** guide! This comprehensive manual will assist you in setting up and mastering the development environment required for working with Ledger devices. Please note that Ledger supports only **Linux** as a development environment, and this guide uses **Ubuntu 20.04** as the base operating system.

## Introduction
Ledger devices are secure hardware wallets that allow users to store, manage, and interact with their cryptocurrencies. Developing applications for these devices involves setting up a well-organized development environment that includes software development kits (SDKs), emulators for testing without real hardware, and essential toolchains.

In this guide, we will:
1. Set up directories and install necessary SDKs.
2. Configure emulators to simulate Ledger devices.
3. Install and configure the required toolchains.
4. Establish a Python virtual environment for development.

By the end of this process, your development environment will look like this:
```plaintext
.
├── app-epicchain
├── clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04
├── gcc-arm-none-eabi-10-2020-q4-major
├── nanos-secure-sdk
├── nanox-secure-sdk
├── setup_nanos.sh
├── setup_nanox.sh
├── speculos
└── venv
```

---

## Prerequisites
Before diving into the setup process, ensure you have the following:
- **Python 3.8 or higher** installed on your system.
- Sufficient permissions to create directories under `/opt`.
- Basic familiarity with Linux terminal commands.

### Step 1: Create the Base Directory
To maintain a clean and organized workspace, create a dedicated folder for Ledger development:
```bash
sudo mkdir -p /opt/ledger
cd /opt/ledger
```

---

## Setting Up the Environment

### Step 2: Clone the Required Repositories
Start by cloning the necessary repositories for the EpicChain app and the Speculos emulator:
```bash
git clone https://github.com/epicchainlabs/epicchain-ledger-nano.git app-epicchain
git clone https://github.com/LedgerHQ/speculos.git
```

### Step 3: Download the SDKs
SDKs are crucial for developing applications compatible with Ledger devices. Clone the official SDK repositories:
```bash
git clone https://github.com/LedgerHQ/nanos-secure-sdk.git
git clone https://github.com/LedgerHQ/nanox-secure-sdk.git
```

### Step 4: Download and Extract the Toolchains
Ledger requires specific toolchains for development, such as **Clang** and **ARM GCC**. Download and extract the required versions:
```bash
wget https://releases.llvm.org/9.0.0/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz
wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/10-2020q4/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2

tar xf clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz
sudo tar xf gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2
```

### Step 5: Install Cross-Compilation Headers
Cross-compilation headers are needed for building applications for multiple architectures. Install these headers using the following command:
```bash
sudo apt install gcc-multilib g++-multilib
```

### Step 6: Configure udev Rules
Setting up udev rules allows your Linux system to communicate with the Ledger device. Run the following script:
```bash
wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```

If your device still fails to connect, manually modify the rules under `/etc/udev/rules.d/20-hw1.rules`. Add your current account as the `OWNER`:
```plaintext
# Nano X
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2c97", ATTRS{idProduct}=="0004|4000|4001|...|401f", TAG+="uaccess", TAG+="udev-acl", OWNER="<your_username>"
```

---

## Python Environment Setup
Both the Speculos emulator and the EpicChain app require Python for communication. Follow these steps to create a Python virtual environment:
```bash
python3 -m venv venv
source venv/bin/activate
pip install python-dev-tools ledgerblue construct jsonschema mnemonic pycrypto pyelftools pbkdf2 pytest Pillow PyQt5 ledgercomm
```
Additionally, install USB-related libraries:
```bash
sudo apt install libudev-dev libusb-1.0-0-dev
```

---

## Configuration Scripts
To simplify the setup process, create environment configuration scripts for Nano S and Nano X devices.

### Step 7: Nano S Configuration Script
Create `setup_nanos.sh`:
```bash
export PATH="/opt/ledger/gcc-arm-none-eabi-10-2020-q4-major/bin:$PATH"
export PATH="/opt/ledger/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04/bin:$PATH"
export BOLOS_SDK="/opt/ledger/nanos-secure-sdk/"
```

### Step 8: Nano X Configuration Script
Create `setup_nanox.sh`:
```bash
export PATH="/opt/ledger/gcc-arm-none-eabi-10-2020-q4-major/bin:$PATH"
export PATH="/opt/ledger/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04/bin:$PATH"
export BOLOS_SDK="/opt/ledger/nanox-secure-sdk/"
```

---

## Building and Testing the App

### Step 9: Building the App
1. Activate the appropriate environment:
   ```bash
   source ./setup_nanos.sh
   ```
2. Enable the Python virtual environment:
   ```bash
   source venv/bin/activate
   ```
3. Navigate to the app directory and build:
   ```bash
   cd app-epicchain
   make
   ```
4. To load the app onto your physical device:
   ```bash
   make load
   ```
Ensure that your Ledger device is unlocked before running `make load`.

### Step 10: Running the Emulator
To test the app without physical hardware, use the Speculos emulator:

#### For Nano S:
```bash
cd speculos
./speculos.py --sdk 2.0 ../app-epicchain/bin/app.elf
```

#### For Nano X:
```bash
./speculos.py -m nanox ../app-epicchain/bin/app.elf
```

**Optional:** Set the `DEFAULT_SEED` in `speculos.py` to match your physical device for consistent outputs. Alternatively, use the `-s` switch to specify the seed during runtime.

---

## Summary
Congratulations! You have successfully set up the development environment for EpicChain on Ledger devices. This environment empowers you to build, test, and deploy secure applications tailored for Nano S and Nano X devices. With your tools and configurations in place, you are now ready to dive into the exciting world of blockchain and cryptocurrency development.

If you encounter any issues or need further assistance, consult the official Ledger documentation or reach out to the EpicChain development community.

Happy coding!

