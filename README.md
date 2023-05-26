<p align="center">
  <img src="./.img/Minikube_logo2.png" style="height: 150px;"/>
</p>

# [Minikube](https://minikube.sigs.k8s.io/docs/start/) 
– minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows.

## 1. Установка Minikube на Linux
Установка Minikube на ubuntu-22.04.2-live-server-amd64 с использованием VirtualBox v7.0.
Эта установка предполагает, что на нашей рабочей станции Linux не установлено никакого другого программного обеспечения для изоляции, включая драйверы KVM2, QEMU, Docker Engine или Podman.

Проверьте поддержку виртуализации в вашей ОС Linux в терминале. 
([Check virtualization support](https://minikube.sigs.k8s.io/docs/drivers/kvm2/#check-virtualization-support) )


**Prerequisites to install minikube:**

1. sudo privileges or root user access
2. 2 CPUs or more
3. 2GB of free memory
4. 20GB of free disk space
5. Internet connection
6. Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation
7. Virtualization must be enabled.
------------------------------------------------------------------------------------------------------------

To use VM drivers, verify that your system has virtualization support enabled:
```
egrep -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no

grep -E --color 'vmx|svm' /proc/cpuinfo
```
If the above command outputs "no" or nothing:

-   If you are running within a VM, your hypervisor does not allow nested virtualization. You will need to use the *None (bare-metal)* driver
-   If you are running on a physical machine, ensure that your BIOS has hardware virtualization enabled 

Or make sure that you have hardware virtualization enabled with the following command:  
```console
lscpu | grep Virtualization

#  Virtualization:                  VT-x
#  Virtualization type:             full
```
If hardware virtualization is enabled, you will see the text AMD-V (For AMD processors) or VT-X/VT-D (For Intel processors) in the Virtualization section. 

⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️  
**Для процессоров Intel вложенная виртуализация недоступна   
из графического интерфейса программы, но ее можно включить   
с помощью командной строки.**  
  
**Как включить вложенную виртуализацию Nested VT-x VirtualBox в системах в Microsoft Windows**  
Вложенная виртуализация включается отдельно для каждой виртуальной системы.  
Открываем Командую строку (cmd.exe) от имени Администратора и выполняем следующие команды.  
Переходим в директорию установленной программы в Program Files:  
```cd C:\Program Files\Oracle\VirtualBox```  
  
Выводим список виртуальных систем с помощью команды:  
```VBoxManage.exe list vms```  
  
Выбрав точное название виртуальной системы, подключаем вложенную виртуализацию:  
```VBoxManage.exe modifyvm "название виртуальной системы" --nested-hw-virt on```  
⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️    

### 1.1. VirtualBox
Самый простой способ загрузить и установить гипервизор VirtualBox для Linux - это с его официального сайта. Однако, если вы хотите поэкспериментировать, можно добавить рекомендуемый репозиторий исходного кода для хостовой ОС, загрузить и зарегистрировать открытый ключ, а затем обновить и установить его непосредственно в терминале.
```console
# Install VirtualBox 7.0 on Ubuntu Server 20.04
# 1. Install system updates
sudo apt update && sudo apt -y upgrade

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

# 5: Check VirtualBox 7.0 on Ubuntu from terminal
virtualbox -h
```

### 1.2. Minikube  
**Install Kubectl**  
Kubectl is a command-line interface for running commands against Kubernetes clusters. It's also required for the installation of Minikube. Run the following commands to install kubectl:

```console
sudo apt update && sudo apt install -y apt-transport-https curl

sudo snap install kubectl --classic  
```

Minikube можно загрузить и установить в терминале. Последняя версия или конкретный релиз доступны на странице релизов Minikube.  
```console
#To download the Minikube DEB package from the official website of Minikube as follows:  
wget -O /tmp/minikube_latest.deb https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

#To install Minikube from the DEB package file minikube_latest.deb, run the following command:
sudo apt install /tmp/minikube_latest.deb

#To check whether Minikube is installed, run the following command:  
minikube version
```
  
**Запуск Minikube.**  
Вы можете запустить Minikube в терминале с помощью команды `minikube start --vm-driver=virtualbox`, которая загрузит одноконечный кластер с последней стабильной версией Kubernetes. Для определенной версии Kubernetes можно использовать параметр `--kubernetes-version`, например, `minikube start --kubernetes-version v1.25.1` (где `latest` является значением по умолчанию, а `stable` также является допустимым значением).

**(VirtualBox)**  
```console
$ minikube start --driver=virtualbox
    * minikube v1.30.1 on Ubuntu 22.04
    * Using the virtualbox driver based on user configuration
    * Downloading VM boot image ...
        > minikube-v1.30.1-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
        > minikube-v1.30.1-amd64.iso:  282.84 MiB / 282.84 MiB  100.00% 6.73 MiB p/
    * Starting control plane node minikube in cluster minikube
    * Downloading Kubernetes v1.26.3 preload ...
        > preloaded-images-k8s-v18-v1...:  397.02 MiB / 397.02 MiB  100.00% 7.19 Mi
    * Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
    * Preparing Kubernetes v1.26.3 on Docker 20.10.23 ...
      - Generating certificates and keys ...
      - Booting up control plane ...
      - Configuring RBAC rules ...
    * Configuring bridge CNI (Container Networking Interface) ...
      - Using image gcr.io/k8s-minikube/storage-provisioner:v5
    E0526 12:58:15.944419    7531 start.go:219] Unable to scale down deployment "coredns" in namespace "kube-system" to 1 replica: non-retryable failure while rescaling coredns deployment: Operation cannot be fulfilled on deployments.apps "coredns": the object has been modified; please apply your changes to the latest version and try again
    * Verifying Kubernetes components...
    * Enabled addons: default-storageclass, storage-provisioner
    * Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

$ minikube status
    minikube
    type: Control Plane
    host: Running
    kubelet: Running
    apiserver: Running
    kubeconfig: Configured

$ minikube stop
    * Stopping node "minikube"  ...
    * 1 node stopped.

# Если вы перезагрузите хост, то повторный запуск Minikube выполняется командой:
minikube start
```
  
Команда minikube profile позволяет просмотреть состояние всех наших кластеров в виде таблицы. Предположим, что мы создали только кластер Minikube по умолчанию, мы можем перечислить свойства, определяющие профиль по умолчанию, с помощью следующей команды:  
```console
$ minikube profile list
|----------|------------|---------|----------------|------|---------|---------|-------|--------|
| Profile  | VM Driver  | Runtime |       IP       | Port | Version | Status  | Nodes | Active |
|----------|------------|---------|----------------|------|---------|---------|-------|--------|
| minikube | virtualbox | docker  | 192.168.59.100 | 8443 | v1.26.3 | Running |     1 | *      |
|----------|------------|---------|----------------|------|---------|---------|-------|--------|
```
Эта таблица содержит столбцы, связанные со стандартными свойствами, такими как 
- имя профиля: minikube, 
- драйвер изоляции: VirtualBox, 
- контейнерная среда выполнения: Docker, 
- версия Kubernetes: v1.25.3, 
- состояние кластера - запущено или остановлено.   
Таблица также предоставляет количество узлов: по умолчанию 1, приватный IP-адрес виртуальной машины управления кластером minikube VirtualBox и защищенный порт, через который API-сервер открыт для компонентов, агентов и клиентов панели управления кластером: 8443.  



### LINKS:
- [Установка Kubernetes с помощью Minikube](https://kubernetes.io/ru/docs/setup/learning-environment/minikube/)
- [Prometheus / The Linux Foundation INTRO101 / Основи Kubernetes](https://apps.prometheus.org.ua/learning/course/course-v1:LinuxFoundation+INTRO101+2023_T1/home)
- [Шпаргалка по kubectl](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/)
- [How To Use minikube for Local Kubernetes Development and Testing](https://www.digitalocean.com/community/tutorials/how-to-use-minikube-for-local-kubernetes-development-and-testing)
- [Install KVM on CentOS / RHEL / Ubuntu / Debian / SLES / Arch Linux](https://computingforgeeks.com/install-kvm-centos-rhel-ubuntu-debian-sles-arch/)
- [Установка Minikube](https://itproblog.ru/%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-minikube/)


### :warning: :warning: :warning: :warning: :warning: :warning: :warning: :warning:
```console
Failed to connect to ubuntu.com meta-release-lts

# Иногда после входа в систему появляется сообщение Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

# Чтобы убрать это сообщение, выполните команду

sudo truncate -s 0 /var/lib/ubuntu-release-upgrader/release-upgrade-available
```