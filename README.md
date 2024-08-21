# BlueSQL Groundwork

이 문서는 BlueSQL의 설치를 위해서 필요한 Linux 패키지들을 설치하는 절차를 기술한다.

> Linux 서버가 폐쇄망에 설치될 경우 수행될 수 없음으로 납품전에 Linux OS의 설치 후에 먼저 수행한 후 납품되어야 한다.

<br>

## 목차
- [BlueSQL Groundwork](#bluesql-groundwork)
  - [목차](#목차)
  - [Linux의 설치](#linux의-설치)
  - [필수 패키지 설치](#필수-패키지-설치)
    - [설치 스크립트 다운로드](#설치-스크립트-다운로드)
    - [설치 스크립트 수행](#설치-스크립트-수행)
  - [Linux 서버 설치 계획](#linux-서버-설치-계획)

<br>

## Linux의 설치

BlueSQL은 아래의 Linux 배포본을 지원한다.

| Distro | Version | 설치 디스크 이미지 다운로드 링크 |
|---|---|---|
| Ubuntu | 22.04 | [Ubuntu Desktop](https://releases.ubuntu.com/22.04/ubuntu-22.04.4-desktop-amd64.iso) / [Ubuntu Server](https://releases.ubuntu.com/22.04/ubuntu-22.04.4-live-server-amd64.iso) |
| Rocky Linux | 9.4 | [Rocky Linux](https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.4-x86_64-dvd.iso) |

> 다음의 페이지에 ["부트 USB 생성"](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick) 방법이 기술되어 있다.

각 Linux 배포본의 설치 절차에 따라 Linux를 먼저 설치한다.

<br>

## 필수 패키지 설치

다음의 절차에 따라 BlueSQL의 설치에 필요한 패키지들의 설치를 수행할 수 있다.

### 설치 스크립트 다운로드
  ~~~
  wget -O /tmp/flattening.run \
  https://raw.githubusercontent.com/BlueSQL/groundwork/master/flattening.run
  ~~~

### 설치 스크립트 수행
  ~~~
  sudo bash /tmp/flattening.run
  ~~~

설치 스크립트의 수행시에는 다음의 항목들을 입력하여야 한다.

| 항목 | | 값 | 비고 |
|---|---|---|---|
| root 암호 변경 유무 | 필수 | Y/N | root의 암호가 없으면 Y, 있으면 N을 입력 |
| 변경될 root 암호 | 선택 | 문자열 | 위의 값이 Y이면 변경할 암호를 2회 입력  |
| root 암호 | 필수 | 문자열 | 스크립트에서 사용할 root 암호를 입력 |
| 설치 후 root 암호 삭제 유무 | 필수 | Y/N | Y이면 스크립트 수행 종료 시 root 암호를 삭제 |
| Docker 패키지 설치 유무 | 필수 | Y/N | Y이면 Docker와 관련한 패키지를 설치 |
| BlueSQL 패키지 다운로드 유무 | 선택 | Y/N | Y이면 BlueSQL 패키지(Docker Image)를 다운로드 |
| Docker 로그인 ID | 선택 | 문자열 | Docker Hub의 로그인 ID |
| Docker 로그인 토큰 | 선택 | 문자열 | Docker Hub의 로그인 토큰 |

> 설치시에 입력이 필요하거나 중요한 텍스트는 <span style="color:green">녹색</span>으로 출력된다. 오류 메시지는 <span style="color:red">적색</span>으로 출력된다.

설치 과정의 예제는 아래와 같다.

  ~~~
  Begin flattening for "ubuntu 22.04"
  ... 중략 ....
  Enable root login over SSH.
  Are you changing your root password? (Y/N)
  Changing password for user root.
  New password:
  Retype new password:
  passwd: all authentication tokens updated successfully.
  Enter root password:
  Do you want to delete root password after flattening? (Y/N)
  Would you like to install Docker? (Y/N)
  Would you like to download BlueSQL Package? (Y/N)
  Enter docker login id:
  Enter docker token:
  Enter BlueSQL version:
  Check root login with passworld over SSH.
  ... 중략 ....
  Warning: Permanently add 'localhost' (ED25519) to the list of known hosts.
  Login Success
  Locale and time zone settings.
  ... 중략 ....
  Install Python 3.10.14.
  ... 중략 ....
  Check python path.
  Python installed: "/opt/pyenv/shims/python3".
  Install Ubuntu packages.
  ... 중략 ....
  Install Python modules.
  ... 중략 ....
  Install Docker packages.
  ... 중략 ....
  Download BlueSQL package.
  ... 중략 ....
  Disable daily messages.
  ... 중략 ....
  Root password has been deleted.
  End flattening for "ubuntu 22.04"
  ~~~

> 마지막 라인이 <span style="color:green">End flattening</span>으로 시작하는 문장이여야 설치가 성공한 것이다. Linux 배포본과 설정에 따라 작은 메시지의 차이는 있을 수 있다.

<br>

## Linux 서버 설치 계획

Linux 서버의 설치에 앞서서 아래의 양식에 따라 서버 구성을 협의한 후 설치를 진행한다.

| 호스트명 | bluesql | node1 | ... | node4 | 
|---|---|---|---|---|
| 관리자 계정 | bluesql | bluesql | ... | bluesql |
| 관리자 암호 | ******** | ******** | ... | ******** |
| 디스크 구성 | nvme0n1 /<br>sda /data | nvme0n1 / | ... | nvme0n1 / |
| IP 주소 | 172.31.0.100/24 | 172.31.0.101/24 | ... | 172.31.0.104/24 |
| Gateway | 172.31.0.254 | 172.31.0.254 | ... | 172.31.0.254 |
| DNS 서버 | 1.1.1.1,8.8.8.8 | 1.1.1.1,8.8.8.8 | ... | 1.1.1.1,8.8.8.8 |
| root 암호 변경 유무 | Y | Y | ... | Y |
| 변경될 root 암호 | ******** | ******** | ... | ******** |
| root 암호 | ******** | ******** | ... | ******** |
| 설치 후 root 암호 삭제 유무 | N | N | ... | N |
| Docker 패키지 설치 유무 | Y | N | ... | N |
| BlueSQL 패키지 다운로드 유무 | Y | | ... | |
| Docker 로그인 ID | docker_id | | ... | |
| Docker 로그인 토큰 | ******** | | ... | |

> 호스트명, 관리자 계정/암호, 디스크 구성과 네트워크 설정은 Linux의 설치 시나 설치 후에 별도로 위의 설정에 따라 설정되어야 한다.<br>
> 스크립트의 수행에는 인터넷의 접속이 필요함으로 일시적으로 인터넷에 접속 가능하도록 네트워크를 구성한 후 스크립트를 수행하고 폐쇄망에 네트워크 구성에 따라 수정할 필요도 있다.<br>
> 상황에 따라 "BlueSQL 패키지 다운로드 유무"는 N로 하고 추후에 별도로 다운로드될 수도 있다.<br>
