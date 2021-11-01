# Keycloak Realm

- realm은 사용자, 자격증명, 역할 및 그룹 집합을 관리합니다.
- realm은 서로 격리되어 있으며 자신이 제어하는 사용자만 관리하고 인증할 수 있습니다.

- keycloak operator를 사용하여 kubernetes manifast를 이용하여 realm을 생성합니다.
- keycloak web에 접속하여 realm을 생성도 가능합니다.

## Realm Create

- keycloak-operator-15.0.1/deploy/examples/realm/basic_realm.yaml을 사용합니다.

- realm name이름을 kubernets로 변경합니다.

  ```yaml
  apiVersion: keycloak.org/v1alpha1
  kind: KeycloakRealm
  metadata:
    name: kubernetes-keycloakrealm
    labels:
      app: sso
  spec:
    realm:
      id: "kubernetes"
      realm: "kubernetes"
      enabled: True
      displayName: "Kubernetes Realm"
    instanceSelector:
      matchLabels:
        app: sso
  ```

  ```bash
  kubectl apply -f basic_realm.yaml -n keycloak
  ```

## Client Create

- .../examples/client/client-secret.yaml을 사용합니다.

- 추가적인 옵션을 설정합니다.

  ```yaml
  apiVersion: keycloak.org/v1alpha1
  kind: KeycloakClient
  metadata:
    name: exampleClient
    labels:
      app: sso
  spec:
    realmSelector:
      matchLabels:
  	    app: sso
    client:
      clientId: exampleClient
    	# openssl rand -hex32 | base64  
      secret: YmI5ZGI2ZWI5NWNmNjk4ZjI3ZDViNTFjYTdiNzk2YzIwOTg1MDNiZDU1YmU5MTE1OWJmNTE4NGNkZGE1MGIzMAo=
      clientAuthenticatorType: public
      protocol: openid-connect
      enabled: true
      redirectUris:
        - [redirect uri]
      rootUrl: [root url]
      adminUrl: [admin url]
      webOrigins: 
        - "*"
      defaultClientScopes:
        - roles
        - email
        - profile
        - web-origins
      optionalClientScopes:
        - address
        - microprofile-jwt
        - offline_access
        - phone
      publicClient: true
      standardFlowEnabled: true
      directAccessGrantsEnabled: true
  ```

  

