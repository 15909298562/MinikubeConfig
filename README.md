# MinikubeConfig
使用minikube学习kubernetes时常用到的配置文件。

# 使用minikube创建minikube集群节点
minikube start -p node-name --driver=docker --kubernetes-version=v1.23.8 --image-mirror-country="cn" --registry-mirror=https://hz4anb2p.mirror.aliyuncs.com --registry-mirror=https://hub-mirror.c.163.com --registry-mirror=https://docker.mirrors.ustc.edu.cn --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers --insecure-registry=registry.cn-hangzhou.aliyuncs.com --insecure-registry=registry.k8s.io --mount-port=22 --mount=true --mount-ip="192.168.1.11" --mount-string='E:\AX\Project\dockerfile\MinikubeLogs:/opt/logs' </br>
说明：</br>
  1、node-name是集群节点的名称。</br>
  2、--mount-port=22表示将节点的22端口映射到宿主机的22端口。</br>
  3、--mount-string='E:\AX\Project\dockerfile\MinikubeLogs:/opt/logs'将节点的资源路径映射到宿主机上。

# 启用dashboard
  minikube -p node-name addons enable dashboard</br>
  minikube -p node-name dashboard</br>
如果dashboard一直启动不起来，大概率是因为镜像拉不下来，这个时候可以使用kubernetes-dashboard.yaml文件来进行创建并启用。

# 启用ingress
  minikube -p node-name addons enable ingress</br>
  minikube -p node-name ingress</br>
如果ingress一直启动不起来，大概率是因为镜像拉不下来，这个时候可以使用kubernetes-ingress.yaml文件来进行创建并启用。

# 创建namespace secret pv pvc注意将名称空间更改为你的名称空间
  kubectl apply -f namespace.yaml,secret.yaml,pv.yaml,pvc.yaml</br>
密钥中配置你的docker仓库用户名和密码，如果你没有私有仓库可以去阿里云注册一个，免费的，地址：https://cr.console.aliyun.com/cn-beijing/instance/dashboard。

# 创建deployment service ingress注意将名称空间更改为你的名称空间
  kubectl apply -f your-deployment.yaml,your-service.yaml,your-ingress.yaml

# kubectl为我们提供代理的模式可以代理集群中的应用
  kubectl proxy --address="192.168.1.11" --port=80 --accept-hosts="^*$" -n namespace-name</br>
我们在集群外部可以通过如下规则访问应用：</br>
  http://[proxy-ip]:[proxy-port]/api/v1/namespaces/[namespace-name]/services/[service-name]/proxy

<hr style="height:2px">
至此已经完成了minikube创建集群节点并启动相关组件的活动，可以在dashboard面板中看到当前名称空间下的所有组件，绿色就表示成功。
