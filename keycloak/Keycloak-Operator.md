# Keycloak Operator

- ID 및 Access 관리를 제공하는 SSO(Single-Sign-On) opensource입니다.
- 이번 실습에서 kubernetes위에 keycloak을 실행하도록 하겠습니다.
- keycloak에서 제공하는 keycloak operator를 사용하여 진행합니다.
- keycloak operator는 kuberentes에서 keycloak 관리를 자동화 합니다.
- operator를 이용하여 다음과 같은 작업을 수행할 수 있습니다.
  1. Install keycloak
  2. Create realms
  3. Create clients
  4. Create users
  5. Connect to an external dtatabase
  6. Schedule database backups
  7. Install extensions and thems

## Operator Install

### keycloak-operator download

- git url: https://github.com/keycloak/keycloak-operator
- 해당 실습에서 15.0.1 version을 사용

```bash
wget https://github.com/keycloak/keycloak-operator/archive/refs/tags/15.0.1.tar.gz
tar zxvf 15.0.1.tar.gz
cd keycloak-operator-15.0.1
```

### namespace 생성 및 operator 배포

- 기본 operator 배포는 두 가지 방법이 있습니다.

  1. keycloak git에서 제공하는 Makefile 사용

     ```bash
     make cluster/prepare
     ```

  2. kubectl apply로 operator 배포

     ```bash
     # namespace 생성
     kubectl apply -f deploy/crds
     kubectl apply -f deploy/namespace.yaml
     
     kubectl apply -f deploy/role.yaml -n keycloak
     kubectl apply -f deploy/role_binding.yaml -n keycloak
     kubectl apply -f deploy/service_account.yaml -n keycloak
     
     kubectl apply -f deploy/operator.yaml -n keycloak
     ```

  3. operator가 실행 중인지 확인

     ```bash
     kubectl get deployment -n keycloak
     ```

## keycloak 설정

### keycloak 실행

- ingress host name 설정 시 externalAccess.host=example.com 추가하시면 됩니다.

  ```bash
  kubectl apply -f deploy/example/keycloak/keycloak.yaml -n keycloak
  ```

- ingress에 tls 추가

  ```bash
  kubectl create secrets tls keycloak-tls -n keycloak --cert=[path] --key=[path]
  
  kubectl edit ingress -n keycloak keycloak
  ...
  spec.tls.0.hosts.0 = example.com
  spec.tls.0.secretName: keycloak-tls
  ...
  ```

### keycloak web access

- https://example.com

- user name  확인

  ```bash
  kubectl get secrets -n keycloak credential-sso-keycloak -o jsonpath='{.data.ADMIN_USERNAME}' | base64 -d && echo
  ```

- user password 확인

  ```bash
  kubectl get secrets -n keycloak credential-sso-keycloak -o jsonpath='{.data.ADMIN_PASSWORD}' | base64 -d && echo
  ```

  

