# Kubernetes 설치
[docker 설치](https://docs.docker.com/engine/install/ubuntu/)
[k8s 공식 문서 참조](https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## 환경
- vagrant를 이용하여 virtualbox로 ubuntu를 사용하여 설치 진행함.
- Controller용 VM 1대, Node용 VM 3대 구성

```
1. docker 설치
2. kubeadm 설치

```
## 노드추가
```
파드는 워커 노드들에서 실행됨.
컨트롤 플레인에서 토큰값은 24시간이 지나면 만료됨. 따라서 노드를 추가시 토큰을 재생성해야함.
kubeadm token create
```


## 업데이트
```
github sh 파일 참조

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
echo "KUBELET_EXTRA_ARGS='--node-ip {노드의 ip}'" | sudo tee /etc/default/kubelet
 ```


### vagrantfile
```
Vagrant.configure("2") do |config|
        config.vm.define "k-control" do |my|
                my.vm.box = "ubuntu/focal64"
                my.vm.hostname = "controller"
                my.vm.network "private_network", ip: "192.168.200.50"
                my.vm.provider "virtualbox" do |vb|
                        vb.name = "control"
                        vb.cpus = 2
                        vb.memory = 3036
                end
        end
        config.vm.define "k-node1" do |my|
                my.vm.box = "ubuntu/focal64"
                my.vm.hostname = "node1"
                my.vm.network "private_network", ip: "192.168.200.51"
                my.vm.provider "virtualbox" do |vb|
                        vb.name = "node1"
                        vb.cpus = 2
                        vb.memory = 3036
                end
        end
         config.vm.define "k-node2" do |my|
                my.vm.box = "ubuntu/focal64"
                my.vm.hostname = "node2"
                my.vm.network "private_network", ip: "192.168.200.52"
                my.vm.provider "virtualbox" do |vb|
                        vb.name = "node2"
                        vb.cpus = 2
                        vb.memory = 3036
                end
        end

        config.vm.define "k-node3" do |my|
                my.vm.box = "ubuntu/focal64"
                my.vm.hostname = "node3"
                my.vm.network "private_network", ip: "192.168.200.53"
                my.vm.provider "virtualbox" do |vb|
                        vb.name = "node3"
                        vb.cpus = 2
                        vb.memory = 3036
                end
        end
end
```
