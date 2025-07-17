## Step

### 安装卸载

```bash
# 安装
curl -sfL https://get.k3s.io | sh -
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | \
INSTALL_K3S_MIRROR=cn sh -s - \
--system-default-registry "https://docker.1ms.run"

# --disable traefik

# 卸载
sudo systemctl stop k3s
sudo systemctl disable k3s

/usr/local/bin/k3s-uninstall.sh

sudo rm -f /usr/local/bin/k3s
sudo rm -f /usr/local/bin/k3s-killall.sh
sudo rm -f /usr/local/bin/k3s-uninstall.sh

sudo rm -rf /var/lib/rancher/k3s
sudo rm -rf /var/lib/kubelet
sudo rm -rf /var/lib/cni
sudo rm -rf /opt/cni

sudo rm -rf /etc/rancher/k3s
sudo rm -rf /etc/systemd/system/k3s*
sudo rm -rf /etc/default/k3s

sudo rm -rf /var/log/rancher
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

### utm虚拟机挂载

```bash
# utm 虚拟机挂载本地目录
mount -t 9p -o trans=virtio share /mnt/mp -oversion=9p2000.L
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
    	#hostPort: 30080 # 映射到节点的 30080 端口
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "256Mi"
        cpu: "200m"
```

- 每个 Pod 在集群内部都有一个唯一的 IP 地址
- describe  port：容器内部监听的端口（即应用实际监听的端口）host port：未映射到主机端口（值为 0 表示禁用）若设置为非零值（如 30080），会将容器端口直接映射到节点的对应端口



## Deployment

### nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 2  # 设置两个副本
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.23
        ports:
        - containerPort: 80
        	#hostPort: 30080      # 每个 Pod 都会映射到节点的 30080 端口,多pod调度器会尝试将它们放在不同节点上，节点不足会导致调度失败
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
```



## Service

### nginx-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort  # 用于外部访问 ClusterIP/NodePort/LoadBalancer
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80        # Service 端口
      targetPort: 80  # Pod 端口
      nodePort: 30080 # 节点端口 (可选，范围 30000-32767)
```

- ClusterIP：**仅集群内部可访问**，通过集群内部 IP 暴露服务，ip不会因为服务器重启变化，删除调整service才会可能改变
- NodePort：**集群内外均可访问**，通过每个节点的 IP 和静态端口（NodePort）暴露服务

### nginx-tomcat-service.yaml

```yaml
# Service A - 访问 nginx Pod
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP
  ports:
    - port: 80          # Service 端口
      targetPort: 80    # Pod 端口
  selector:
    app: nginx          # 选择标签为 app=nginx 的 Pod

# Service B - 访问 tomcat Pod
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service
spec:
  type: ClusterIP
  ports:
    - port: 80          # 与 Service A 相同的 port
      targetPort: 8080  # Tomcat 默认端口
  selector:
    app: tomcat         # 选择标签为 app=tomcat 的 Pod
```

单服务器同一个端口，LoadBalancer 会拦截nginx请求



## Ingress

### nginx-ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```

