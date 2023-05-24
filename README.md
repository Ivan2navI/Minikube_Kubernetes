<p align="center">
  <img src="./.img/Minikube_logo2.png" style="height: 150px;"/>
</p>

# [Minikube](https://minikube.sigs.k8s.io/docs/start/) 
– minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows.

## 1. Установка Minikube на Linux
Установка Minikube на Ubuntu Linux 20.04 LTS с использованием VirtualBox v7.0.
Эта установка предполагает, что на нашей рабочей станции Linux не установлено никакого другого программного обеспечения для изоляции, включая драйверы KVM2, QEMU, Docker Engine или Podman.

Проверьте поддержку виртуализации в вашей ОС Linux в терминале. 
([Check virtualization support](https://minikube.sigs.k8s.io/docs/drivers/kvm2/#check-virtualization-support) )
------------------------------------------------------------------------------------------------------------

To use VM drivers, verify that your system has virtualization support enabled:
```
egrep -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no
```
If the above command outputs "no":

-   If you are running within a VM, your hypervisor does not allow nested virtualization. You will need to use the *None (bare-metal)* driver
-   If you are running on a physical machine, ensure that your BIOS has hardware virtualization enabled  

⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️  
**Для процессоров Intel вложенная виртуализация недоступна   
из графического интерфейса программы, но ее можно включить   
с помощью командной строки.**  
⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️  

**Как включить вложенную виртуализацию Nested VT-x VirtualBox в системах в Microsoft Windows**  
Вложенная виртуализация включается отдельно для каждой виртуальной системы.  
Открываем Командую строку (cmd.exe) от имени Администратора и выполняем следующие команды.  
Переходим в директорию установленной программы в Program Files:  
```cd C:\Program Files\Oracle\VirtualBox```  
  
Выводим список виртуальных систем с помощью команды:  
```VBoxManage.exe list vms```
  
Выбрав точное название виртуальной системы, подключаем вложенную виртуализацию:  
```VBoxManage.exe modifyvm "название виртуальной системы" --nested-hw-virt on```
  

### 1.1. VirtualBox
Самый простой способ загрузить и установить гипервизор VirtualBox для Linux - это с его официального сайта. Однако, если вы хотите поэкспериментировать, можно добавить рекомендуемый репозиторий исходного кода для хостовой ОС, загрузить и зарегистрировать открытый ключ, а затем обновить и установить его непосредственно в терминале.
```console
# Install VirtualBox 7.0 on Ubuntu Server 20.04
# 1. Install system updates
sudo apt update && sudo apt -y upgrade \
[ -f /var/run/reboot-required ] && sudo reboot -f

# 2. Import VirtualBox GPG Keys
#Download
curl https://www.virtualbox.org/download/oracle_vbox_2016.asc | gpg --dearmor > oracle_vbox_2016.gpg
curl https://www.virtualbox.org/download/oracle_vbox.asc | gpg --dearmor > oracle_vbox.gpg

#Install on system
sudo install -o root -g root -m 644 oracle_vbox_2016.gpg /etc/apt/trusted.gpg.d/
sudo install -o root -g root -m 644 oracle_vbox.gpg /etc/apt/trusted.gpg.d/

# 3. Add VirtualBox 7.0 Repository
echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

# 4. Install VirtualBox 7.0 & Extension pack
sudo apt update
sudo apt install linux-headers-$(uname -r) dkms
sudo apt install virtualbox-7.0

# Download VirtualBox Extension Pack
cd ~/
VER=$(curl -s https://download.virtualbox.org/virtualbox/LATEST.TXT)
wget https://download.virtualbox.org/virtualbox/$VER/Oracle_VM_VirtualBox_Extension_Pack-$VER.vbox-extpack

# Once the file is downloaded, install the extension pack using CLI:
sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-*.vbox-extpack

# 5: Launch VirtualBox 7.0 on Ubuntu from terminal
virtualbox
```

### 1.2. KVM
KVM (for Kernel-based Virtual Machine) is a full virtualization solution for Linux on x86 hardware containing virtualization extensions (Intel VT or AMD-V).  

It consists of a loadable kernel module, kvm.ko, that provides the core virtualization infrastructure and a processor specific module, kvm-intel.ko or kvm-amd.ko. It has received huge adoption on enterprise over the last few years.  

For the Ubuntu system, all packages required to run KVM are available on official upstream repositories.  
Install them using the commands:  
```console
sudo apt update
sudo apt -y install qemu-kvm libvirt-dev bridge-utils libvirt-daemon-system libvirt-daemon virtinst bridge-utils libosinfo-bin libguestfs-tools virt-top

# Load and enable the module `vhost-net`.

sudo modprobe vhost_net
sudo lsmod | grep vhost

    vhost_net              32768  0
    vhost                  49152  1 vhost_net
    tap                    24576  1 vhost_net

echo "vhost_net" | sudo tee -a /etc/modules
```

### 1.3. Minikube
Minikube можно загрузить и установить в терминале. Последняя версия или конкретный релиз доступны на странице релизов Minikube.  
```console
$ curl -LO \
  https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

$ sudo dpkg -i minikube_latest_amd64.deb
```
  
**Запуск Minikube.**
Вы можете запустить Minikube в терминале с помощью команды minikube start, которая загрузит одноконечный кластер с последней стабильной версией Kubernetes. Для определенной версии Kubernetes можно использовать параметр --kubernetes-version, например, minikube start --kubernetes-version v1.25.1 (где latest является значением по умолчанию, а stable также является допустимым значением).

**(VirtualBox)**
```console
* minikube v1.30.1 on Ubuntu 20.04
* Automatically selected the virtualbox driver. Other choices: ssh, none
* Downloading VM boot image ...
    > minikube-v1.30.1-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.30.1-amd64.iso:  282.84 MiB / 282.84 MiB  100.00% 7.26 MiB p/
* Starting control plane node minikube in cluster minikube
* Downloading Kubernetes v1.26.3 preload ...
    > preloaded-images-k8s-v18-v1...:  397.02 MiB / 397.02 MiB  100.00% 7.18 Mi
* Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
! StartHost failed, but will try again: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory
* Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
* Failed to start virtualbox VM. Running "minikube delete" may fix it: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory

X Exiting due to HOST_VIRT_UNAVAILABLE: Failed to start host: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory
* Suggestion: Virtualization support is disabled on your computer. If you are running minikube within a VM, try '--driver=docker'. Otherwise, consult your systems BIOS manual for how to enable virtualization.
* Related issues:
  - https://github.com/kubernetes/minikube/issues/3900
  - https://github.com/kubernetes/minikube/issues/4730
```



**(KVM)**
```console
minikube start

* minikube v1.30.1 on Ubuntu 20.04 (vbox/amd64)
* Automatically selected the qemu2 driver. Other choices: ssh, none
* Automatically selected the builtin network
! You are using the QEMU driver without a dedicated network, which doesn't support `minikube service` & `minikube tunnel` commands.
* Downloading VM boot image ...
    > minikube-v1.30.1-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.30.1-amd64.iso:  282.84 MiB / 282.84 MiB  100.00% 6.30 MiB p/
* Starting control plane node minikube in cluster minikube
* Downloading Kubernetes v1.26.3 preload ...
    > preloaded-images-k8s-v18-v1...:  397.02 MiB / 397.02 MiB  100.00% 5.50 Mi
* Creating qemu2 VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
* Deleting "minikube" in qemu2 ...
! StartHost failed, but will try again: creating host: create host timed out in 36  0.000000 seconds
* Creating qemu2 VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
* Failed to start qemu2 VM. Running "minikube delete" may fix it: creating host:  
create: waiting: Maximum number of retries (60) exceeded

X Exiting due to GUEST_PROVISION: error provisioning guest: Failed to start host:                                                                                                                         
creating host: create: waiting: Maximum number of retries (60) exceeded
*
╭─────────────────────────────────────────────────────────────────────────────────────────────╮
│                                                                                             │
│    * If the above advice does not help, please let us know:                                 │
│      https://github.com/kubernetes/minikube/issues/new/choose                               │
│                                                                                             │
│    * Please run `minikube logs --file=logs.txt` and attach logs.txt to the GitHub issue.    │
│                                                                                             │
╰─────────────────────────────────────────────────────────────────────────────────────────────╯

```



### LINKS:
- [Установка Kubernetes с помощью Minikube](https://kubernetes.io/ru/docs/setup/learning-environment/minikube/)
- [Prometheus / The Linux Foundation INTRO101 / Основи Kubernetes](https://apps.prometheus.org.ua/learning/course/course-v1:LinuxFoundation+INTRO101+2023_T1/home)
- [Шпаргалка по kubectl](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/)
- [How To Use minikube for Local Kubernetes Development and Testing](https://www.digitalocean.com/community/tutorials/how-to-use-minikube-for-local-kubernetes-development-and-testing)
- [Install KVM on CentOS / RHEL / Ubuntu / Debian / SLES / Arch Linux](https://computingforgeeks.com/install-kvm-centos-rhel-ubuntu-debian-sles-arch/)


### :warning: :warning: :warning: :warning: :warning: :warning: :warning: :warning:
```console
Failed to connect to ubuntu.com meta-release-lts

# Иногда после входа в систему появляется сообщение Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

# Чтобы убрать это сообщение, выполните команду

sudo truncate -s 0 /var/lib/ubuntu-release-upgrader/release-upgrade-available
```