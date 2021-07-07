# Kubernetes 설치
[docker 설치](https://docs.docker.com/engine/install/ubuntu/)
[k8s 공식 문서 참조](https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## 환경
- vagrant를 이용하여 virtualbox로 ubuntu를 사용하여 설치 진행함.
- Controller용 VM 1대, Node용 VM 3대 구성

```
1. docker 설치


```



## 업데이트
```
```

## 삭제
```
kubeadm reset
후 관련 디렉토리를 전부 삭제해야함.
kubelet도 삭제

controller
kubectl delete node {nodename}
```


## logs, portforward안되는 문제
```
ehco "KUBELET_EXTRA_ARGS='--node-ip {노드의 ip}'"
| sudo tee /etc/default/kubelet 
 ```
