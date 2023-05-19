[Prometheus / The Linux Foundation INTRO101 / Основи Kubernetes](https://apps.prometheus.org.ua/learning/course/course-v1:LinuxFoundation+INTRO101+2023_T1/home)

<p align="center">
  <img src="./.img/Minikube_logo.png">
</p>

**Minikube** – це один із найпростіших, найгнучкіших та найпопулярніших методів запуску «все-в-одному» або кількома робочими нодами локального кластера Kubernetes, ізольованого віртуальними машинами (ВМ) або контейнерами безпосередньо на робочих станціях.   

Minikube – це інструмент для встановлення компонентів Kubernetes, завантаження кластера або його демонтажу. Він містить також додаткові функції, спрямовані на полегшення взаємодії користувача з кластером Kubernetes, але, тим не менш, ініціалізує також повністю функціональний невиробничий кластер, надзвичайно корисний для навчання. Minikube можна встановити на macOS, Windows та на багатьох дистрибутивах Linux.


Failed to connect to ubuntu.com meta-release-lts

Иногда после входа в систему появляется сообщение Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Чтобы убрать это сообщение, выполните команду

sudo truncate -s 0 /var/lib/ubuntu-release-upgrader/release-upgrade-available

