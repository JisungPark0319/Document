# Ansible

- IT 자동화 도구
- 시스템을 구성하고 소프트웨어를 배포
- 지속적 배포(CD)나 다운타임 제로 업데이트와 같은 고급 IT 작업을 조정
- 전달을 위해서 OpenSSH 사용
- Ansible은 에이전트 없이 시스템을 관리
  - 원격 데몬(daemon)을 업그레이드하는 방법이나 데몬이 제거돼 시스템을 관리 할 수 없는 문제가 없다
- Control node
  - ansible이 설치된 모든 시스템
  - 어떤 제어 노드에서든 /usr/bin/ansible or /usr/bin/ansible-playbook 호출함으로써 command와 playbook을 실행할 수 있다.
- Managed nodes
  - Ansible로 관리하는 네트워크 장치 (or Servers)
- Inventory
  - 관리되는 노드 목록
  - 인벤토리 파일은 때때로 호스트 파일이라고도 한다.
  - 인벤토리는 관리되는 노드에 대한 각각의 IP주소와 같은 정보를 지정할 수 있다.
- Modules
  - Ansible 코드의 실행 단위
  - 각 모듈은 특정 유형의 데이터베이스에서 사용자를 관리하는 것부터 특정 유형의 네트워크 장치에서 VLAN 인터페이스 관리까지 특정한 용도로 사용
- Tasks
  - Ansible의 작업 단위
- Playbooks
  - 반복해서 실행하고자 해당 작업을 실행 순서대로 저장해 놓은 정렬된 작업 리스트



## 구성요소

### Inventory

- 프로비저닝, 배포 등의 대상을 정리한 파일입니다.
- 쉽게 host ip 들을 정리 해놓은 파일이고 별명을 붙이거나 그룹으로 묶거나 혹은 ssh 접근 방식을 기록해 놓을 수 있습니다.

```yaml
[a]
server0 ansible_host=127.0.0.1 ip=192.168.100.10
server1 ansible_host=172.10.0.1 ansible_port=20001

[b]
server3 ansible_host=172.10.2.1 ansible_port=20001

[c:children]
a
b
```

### Playbook

- Inventory에 작성된 서버들을 대상으로 특정 행위(프로비저닝, 배포 등)에 대해 정의한 파일입니다.

```yaml
---
- hosts: all
  become: true
  vars_files:
    - vars/default.yml

  tasks:
    - name: Install yum utils
      yum:
        name:
          - yum-utils
        state: latest
    - name: Service firewall start
      service:
        name: firewalld
        state: started
```

### 추가적인 directory 구조

1. group_vars
   - 공통적으로 사용되는 변수파일들의 directory입니다.
2. roles
   - 동작에 대해 task 단위로 정의해 놓은 파일들의 directory입니다.

## Ansible Install

- 설치 버전: centos7.9
- package tool: yum
- user: root

```bash
# Ansible install를 위해 사전 package 설치
yum install -y epel-release

# Ansible install
yum install -y ansible
```

## Ansible example

- 사용 전 Ansible host에 서버 ip를 정의해야합니다.
- /etc/ansible/hosts
- 192.168.0.10, 192.168.0.11, 192.168.0.12를 하나의 test라는 그룹으로 묶어 프로비저닝

```bash
vi /etc/ansible/hosts
# 맨 아래에 작성
...
[test]
192.168.0.10
192.168.0.11
192.168.0.12

:wq!
```

- ssh key 설정

```bash
ssh-keygen
...
ssh-copy-id root@ip
...
```

- server의 ping 테스트

```bash
# ansible [프로비저닝할 host/그룹] -m 모듈 -a "모듈 옵션"
ansible test -m ping
# all 사용 시 모든 그룹에 프로비저닝
ansible all -m ping
```

- server의 shell 테스트

```bash
ansible test -m shell -a "df -h"
```