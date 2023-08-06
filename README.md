# Azerothcore-k8s

该项目将Azerothcore部署在Kubernetes上.

### 构建k8s上用的镜像
azerothcore默认的docker部署中，镜像不包含可运行程序，如authserver，worldserver和dbimport，也不包含地图文件。所以需要把这些内容打包到镜像中方便在k8s部署的时候使用
```
docker run --name p-azero --rm -itd -v azerothcore-wotlk_ac-bin-dev:/tmp/bin:ro acore/ac-wotlk-worldserver-local:master bash
docker cp env/docker/data p-azero:/azerothcore/env/dist
docker exec -it p-azero bash
cp /tmp/bin/authserver env/dist/bin
cp /tmp/bin/worldserver env/dist/bin
cp /tmp/bin/dbimport env/dist/bin
exit
docker commit p-azero hellof20/p-azero:v1
```

### 部署MySQL
```
kubectl apply -f mysql.yaml
```

### 填充MySQL
```
kubectl apply -f dbimport.yaml
```

### 部署authserver/worldserver/sdk
```
kubectl apply -f authserver.yaml
kubectl apply -f worldserver.yaml
kubectl apply -f sdk.yaml
```

### 查看IP
```
kubectl get svc
```
