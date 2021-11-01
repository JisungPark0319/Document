# Ansible - Docker Install

- Ansible Playbook을 이용한 Docker install
- directory 구조
  - docker-playbook
    - playbook.yml          # docker isntall 하기 위한 yml 선언문
    - vars
      - default.yml        # docker version을 저장하고 있는 value 선언문
- ansible install 문서
  - https://www.notion.so/Ansible-install-243f9a6c4cbc47bea75b4755e6f85ecc

### playbook.yml

```yaml
---
- hosts: all
  become: true
  vars_files:
    - vars/default.yml

  tasks:
    - name: Install base system package
      yum:
        name:
          - yum-utils
        state: latest

    - name: Firewalld disalbe/stop
      ansible.builtin.service:
        name: firewalld
        enabled: no
        state: stopped

    - name: Add docker to yum repository
      shell: yum-config-manager --add-repo <https://download.docker.com/linux/centos/docker-ce.repo>
      args:
        creates: /etc/yum.repos.d/docker-ce.repo
    
    - name: Install docker package
      yum:
        name:
          - docker-ce-{{ docker_version }}
          - docker-ce-cli-{{ docker_version }}
          - containerd.io
        state: present
    
    - name: Enable/Start the Docker Service
      ansible.builtin.service:
        name: docker
        enabled: yes
        state: started
```

### vars/default.yml

```yaml
---
docker_version: 20.10.7
```

## 실행 방법

```bash
# docker playbook이 있는 directory로 이동
cd docker-playbook
ansible-playbook playbook.yml
```