
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
