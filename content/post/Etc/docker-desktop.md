+++
author = "Seorim"
title = '서버 세팅 : Ubuntu Server에서 Docker Desktop을 돌려보자'
slug = 'docker-desktop'
date = 2023-12-20T16:53:42+09:00
categories = [
    "Server", 
]
tags = [
    "Server setup", "Docker", "Docker Desktop", "Ubuntu Desktop"
]
+++

<style>
g1 { color: #79AC78 }
g2 { color: #B0D9B1 }
g3 { color: #D0E7D2 }
g4 { color: #618264 }
o1 { color: #F9B572 }
w1 { color: #FAF8ED }
</style>

# 서버 세팅

## 계기

부트캠프 과정에서 도커를 사용하는데, 나는 아주 작고 소중한 8GM 메모리를 가진 맥북을 사용하기 때문에 도커 데스크탑을 설치해서 작동시키기가 쉽지 않았다.

Superset을 쓸 때는 Preset.io 서비스를, Airflow를 쓸 때는 Airflow 서버를 만들었다. 하지만 이번엔 도커 그 자체를 사용하는 과정이어서 다른 서비스로 회피할 수 없었다.(ㅠㅠ)
처음에는 우분투 서버에 도커만 설치할까 하다가, 우분투 데스크탑을 설치해 GUI환경을 세팅하고 그 위에 도커 데스크탑을 설치하여 GUI환경 전체를 세팅해보기로 결심했다.

첫날 온갖 시행착오를 겪으며 서버를 세팅한 데는 5시간 정도 걸렸지만, 그 이후 다른 사람에게 알려주면서 같이 세팅했을 땐 한시간~한시간 반 정도 걸렸다. 서버 세팅법을 알려주고 다시 만들어보면서 나름 정리가 되어서, 블로그에 적어보기로 했다.

## 과정

> 서버 설정과 연결은 로컬 터미널에서 진행하며, `google cloud sdk`가 설치되어 있어야 한다.  
> VM 인스턴스에 연결됐을때는 root 계정이 아니라 일반 계정으로 설치 및 설정을 진행한다.

### GCE(Google Compute Engine) VM 설정

> `중첩된 가상화`를 설정해야하기 때문에, 이를 지원하는 머신인 `n1, n2, c3` 중에 골라 사용해야 한다.  
> 도커 데스크탑의 요구 사양이 높은 편이기 때문에, `vCPU 최소 2개 이상, 메모리 최소 8GB 이상`으로 구성해야 한다.

**<o1>1. 머신 및 디스크 사양</o1>**

-   머신 : n1-standard-4 (`4 vCPUs`, 2 cores, `15GB memory`)

-   부팅 디스크 : `Ubuntu 22.04 LTS`, 64GB `SSD`

**<o1>2. 중첩된 가상화 (nested virtualization) 설정</o1>**

`VM_NAME` : 본인의 VM instance 이름

`ZONE` : 해당 instance가 위치한 지역

-   다음 명령어로 VM 속성을 config.yaml 파일으로 내보낸다.

    ```bash
    gcloud compute instances export VM_NAME --destination=config.yaml --zone=ZONE
    ```

-   config.yaml 파일을 열고 다음을 파일에 추가한다.

    ```yaml
    advancedMachineFeatures:
        enableNestedVirtualization: true
    ```

-   다음 명령어로 수정된 config.yaml 파일으로부터 VM 속성을 업데이트 한다.

    ```bash
    gcloud compute instances update-from-file VM_NAME --source=config.yaml --most-disruptive-allowed-action=RESTART --zone=ZONE
    ```

-   업데이트 이후 VM에 연결한다.

    ```bash
    gcloud compute ssh VM_NAME
    ```

-   (연결된 인스턴스에서) 다음 커맨드로 중첩된 가상화가 설정되었는지 확인한다. 0 이외의 응답이 나와야 사용 설정이 제대로 된 것이다.

    ```bash
    grep -cw vmx /proc/cpuinfo
    # 내 경우에는 응답으로 8이 나왔다.
    ```

<https://cloud.google.com/compute/docs/instances/nested-virtualization/enabling?hl=ko#gcloud_1>

**<o1>3. kvm 지원 여부를 확인</o1>**

-   (연결된 VM 인스턴스에서) 다음 커맨드를 입력해 kvm를 지원하는지 확인한다.

    ```bash
    modprobe kvm

    # 위 커맨드가 실패한 경우, 다음 커맨드를 입력해본다.
    kvm-ok

    # 다음 커맨드로 지원되는 kvm module을 확인할 수 있다.
    lsmod | grep kvm
    ```

    <https://docs.docker.com/desktop/install/linux-install/#system-requirements>

### Chrome, Chrome remote desktop, Ubuntu Desktop 설치

<https://ubuntu.com/blog/launch-ubuntu-desktop-on-google-cloud>

<https://snowdeer.github.io/linux/2017/08/04/install-gui-desktop-on-ubuntu/>

**<o1>크롬 설치</o1>**

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

**<o1>크롬 원격 데스크탑 설치</o1>**

```bash
wget https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb
```

```bash
sudo apt-get install --assume-yes ./chrome-remote-desktop_current_amd64.deb
```

**<o1>우분투 데스크탑 설치</o1>**

```bash
sudo apt-get update
sudo apt-get upgrade
apt-get install ubuntu-desktop
```

**<o1></o1>**

### Chrome remote desktop으로 Ubuntu 서버에 접속

**<o1>파일 및 계정 설정</o1>**

-   크롬 원격 데스크탑 실행 시, 우분투 데스크탑이 실행되도록 파일 설정

    <https://carrie2120.tistory.com/2>

-   ubuntu user password 변경 (본인이 자주 사용하는걸로 설정)

**<o1>크롬 원격 데스크탑 연결</o1>**

-   크롬 원격 데스크탑 SSH 연결을 누르고 해당 커맨드 입력
    -   <https://remotedesktop.google.com/headless>
    -   PIN 6자리 설정
    -   이 커맨드는 서버의 원격 데스크탑 호스트를 데몬서비스로 백그라운드에서 실행해주기 때문에, 호스트 서비스를 실행하기 위해 따로 명령어를 실행할 필요가 없음
-   서버 접속 : <https://remotedesktop.google.com/access>
    -   앞에서 설정한 PIN 입력

**<o1>기타 접속을 위한 설정</o1>**

-   ubuntu desktop 실행 시 가끔 발생하는 오류를 막기 위해 extension 설치
    <https://askubuntu.com/questions/1370068/ubuntu-now-always-starts-in-overview-mode-when-logging-in-how-to-avoid>

### Docker Desktop 설치 및 실행

**<o1>Docker 설치</o1>**

<https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository>

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```bash
sudo apt-get update
```

**<o1>Docker desktop 설치</o1>**

<https://docs.docker.com/desktop/install/ubuntu/#install-docker-desktop>

-   xfce 같은 non-Gnome Desktop 환경인 경우에 다음 패키지를 다운받아야 함

```bash
sudo apt install gnome-terminal
```

-   docker-desktop 패키지의 버전은 시간에 따라 달라질 수 있으니 페이지에서 다운받으라는 파일으로 알아서 변경할 것

```bash
sudo apt-get update
```

```bash
wget https://desktop.docker.com/linux/main/amd64/docker-desktop-4.26.1-amd64.deb
```

```bash
sudo apt-get install ./docker-desktop-4.26.1-amd64.deb
```

-   설치 후 application에서 실행하거나, `systemctl --user start docker-desktop`으로 실행

-   시작 시 docker-desktop이 실행되도록 하려면 `systemctl --user enable docker-desktop`으로 설정

**<o1>kvm 권한 부여</o1>**

```bash
# /dev/kvm 권한 확인
ls -al /dev/kvm
# 유저가 kvm에 접근할 수 있도록 kvm 그룹에 추가
sudo usermod -aG kvm $USER
```

### 참고 링크들
