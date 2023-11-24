# Homelab
Repository with instructions and set-up of homelab

## Proxmox VM
The Debian Virtual Machine runs on a proxmox node in the home cluster.
Below the hardware specifications of `proxmox-srv`:

**Hardware specifications**
- **CPU**:  Intel Core i5-6500T CPU @ 2.50GHz
- **RAM**: 32GB
- **Storage**:
  - NVME 1TB
    - Root
    - LVM
  - HDD 500GB
    - ZFS
- **PCI**: Coral AI accelerator

### VM setup
1. Install Debian
2. Add passthrough of PCI devices
3. Enable SSH access
- Edit SSH config
```bash
nano /etc/ssh/sshd_config
```
- Uncomment and edit the following lines 
```nano
PermitRootLogin yes
PasswordAuthentication yes
```
4. Install intel drivers
https://geekistheway.com/2022/12/23/setting-up-intel-gpu-passthrough-on-proxmox-lxc-containers/

5. Install coral drivers
- Add repositories and install drivers
```bash
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | tee /etc/apt/sources.list.d/coral-edgetpu.list

curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

apt update

apt install gasket-dkms libedgetpu1-std
```
- Reboot the system
```bash
reboot now
```
- Check the successful install
```bash
lspci -nn | grep 089a
```
- You should see something like this:
```bash
03:00.0 System peripheral: Device 1ac1:089a
```
- Also verify that the PCIe driver is loaded:
```bash
ls /dev/apex_0
```



