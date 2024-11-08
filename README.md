
```bash
# pod 확인
$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-test-686f6ff54d-gkk9h   1/1     Running   0          29m

# pod 상세정보 - 도커 컨테이너 정보
$ kubectl describe pod nginx-test-686f6ff54d-gkk9h

# 해당 도커 컨테이너로 접속
# $ kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
$ kubectl exec -it nginx-test-686f6ff54d-gkk9h -c nginx-test -- /bin/bash

# 로그확인
$ kubectl logs -f nginx-test-686f6ff54d-gkk9h -c nginx-test
```


```bash
# 생성 및 변경 적용
$ kubectl apply -f httpd-deployment.yaml
deployment.apps/httpd-deployment created
service/httpd-service created

# 배포로 생성된 Pod를 나열
$ kubectl get pods -l app=httpd

# 상세정보
$ kubectl describe pod httpd-deployment-599bf897b4-m9vn5

# 삭제
$ kubectl delete deployment nginx-deployment

```

## config
```bash
$ kubectl apply -f nginx-config.yaml

$ kubectl get configmap -n default

$ kubectl describe configmap nginx-config

```

## helm
- https://helm.sh/docs/intro/cheatsheet/
```bash
$ helm create vninx
$ cd vnginx
$ rm -rf templates/*
$ copy $CODE_HOME/*.yaml ngpd/templates/
$ helm install app . --set httpd.image.tag=2.4.54 --set nginx.image.tag=1.27.1 --set httpd.service.type=NodePort --set nginx.service.type=NodePort
$ helm list
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
app     default         1               2024-11-08 14:42:32.054075154 +0900 KST deployed        vninx-0.1.0     1.16.0
$ helm status app
NAME: app
LAST DEPLOYED: Fri Nov  8 14:42:32 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
$ helm history app
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Fri Nov  8 14:37:23 2024        superseded      vninx-0.1.0     1.16.0          Install complete

$ helm upgrade app .
$ helm list
$ helm history app
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Fri Nov  8 14:37:23 2024        superseded      vninx-0.1.0     1.16.0          Install complete
2               Fri Nov  8 14:41:41 2024        superseded      vninx-0.1.1     1.16.0          Upgrade complete
$ helm rollback app 1
$ helm rollback app 2
$ helm history app
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Fri Nov  8 14:37:23 2024        superseded      vninx-0.1.0     1.16.0          Install complete
2               Fri Nov  8 14:41:41 2024        superseded      vninx-0.1.1     1.16.0          Upgrade complete
3               Fri Nov  8 14:42:22 2024        superseded      vninx-0.1.0     1.16.0          Rollback to 1
4               Fri Nov  8 14:42:32 2024        deployed        vninx-0.1.1     1.16.0          Rollback to 2
$ helm delete app
```

# Helm repo 만들기
```bash
# package 만들기
$ helm package vnginx
Successfully packaged chart and saved it to: /home/diginori/code/docker/k2s/pkg/page/vnginx-0.1.0.tgz
$ helm repo index .
$ ls
index.yaml  vnginx  vnginx-0.1.0.tgz

# git push & github pages 설정

# helm repo 등록
# https://dmario24.github.io/k2s/pkg/page/index.yaml
$ helm repo add my-repo https://dmario24.github.io/k2s/pkg/page
"my-repo" has been added to your repositories


# repo 조회
$ helm repo list
my-repo                 https://dmario24.github.io/k2s/pkg/page

# chart 조회
$ helm search repo my-repo
NAME            CHART VERSION   APP VERSION     DESCRIPTION
my-repo/vnginx  0.1.0           1.16.0          A Helm chart for Kubernetes

# 설치
# $ helm install <설치이름> my-repo/vnginx
$ helm install kingkong my-repo/vnginx
$ helm install superman my-repo/vnginx
```
