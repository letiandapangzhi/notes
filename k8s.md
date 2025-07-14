## Step

### 安装

```bash
# 安装
curl -sfL https://get.k3s.io | sh -
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | \
INSTALL_K3S_MIRROR=cn sh -s - \
--system-default-registry "registry.cn-hangzhou.aliyuncs.com" --write-kubeconfig-mode 644 

```

### 代理

```bash
export http_proxy=http://192.168.1.228:9910
```

### K3s sudo 修正

```bash
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
echo "export KUBECONFIG=~/.kube/config" >> ~/.bashrc
source ~/.bashrc  # 立即生效
```

### 源

```bash
# https://docs.k3s.io/zh/installation/private-registry
vim /etc/rancher/k3s/registries.yaml
```

```yaml
mirrors:
  docker.io:
    endpoint:
      - "https://docker.1ms.run"
configs:
  "registry.example.com:5000":
    tls:
      cert_file: # path to the cert file used in the registry
      key_file:  # path to the key file used in the registry
      ca_file:   # path to the ca file used in the registry
```



## Pod

### nginx-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx:1.23
    ports:
    - containerPort: 80
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "256Mi"
        cpu: "200m"
```

