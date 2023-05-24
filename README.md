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

### 1.2. Minikube
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