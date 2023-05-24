<p align="center">
  <img src="./.img/Minikube_logo2.png" style="height: 150px;"/>
</p>

# [Minikube](https://minikube.sigs.k8s.io/docs/start/) 
– minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows.

## 1. Інсталяція Minikube на Linux
Встановлення Minikube на Ubuntu Linux 20.04 LTS з VirtualBox v7.0.   
Це інсталювання передбачає, що на нашій робочій станції Linux не встановлено жодного іншого програмного забезпечення для ізоляції, зокрема драйверів KVM2, QEMU, Docker Engine або Podman.  

Перевірте підтримку віртуалізації у вашій ОС Linux у терміналі (непорожній вивід вказує на підтримку віртуалізації): 

```$ grep -E --color 'vmx|svm' /proc/cpuinfo```

### 1.1. VirtualBox
Найпростіший спосіб завантажити і встановити гіпервізор VirtualBox для Linux – з його офіційного сайту. Однак, якщо хочеться пригод, можна додати рекомендований репозиторій вихідного коду для хостової ОС, завантажити і зареєструвати відкритий ключ, оновити і встановити його безпосередньо в терміналі: 

$ sudo bash -c 'echo "deb \
  [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] \
  https://download.virtualbox.org/virtualbox/debian \
  eoan contrib" >> /etc/apt/sources.list'

$ wget -O- \
  https://www.virtualbox.org/download/oracle_vbox_2016.asc | \
  sudo gpg --dearmor --yes \
  --output /usr/share/keyrings/oracle-virtualbox-2016.gpg

$ sudo apt update

$ sudo apt install -y virtualbox-7.0


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
Minikube можна завантажити і встановити в терміналі, остання версія або конкретний реліз доступні на сторінці релізів Minikube:
```console
$ curl -LO \
  https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

$ sudo dpkg -i minikube_latest_amd64.deb
```

**Запуск Minikube. (KVM)**
Ви можете запустити Minikube у терміналі за допомогою команди **minikube start**, яка завантажить однонодовий кластер з останнім стабільним релізом Kubernetes. Для конкретного релізу Kubernetes можна використати параметр **--kubernetes-version**, наприклад, **minikube start --kubernetes-version v1.25.1** (де **latest** є значенням за замовчуванням, і **stable** також є прийнятним).

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