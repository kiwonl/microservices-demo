# GKE Autopilot 클러스터 생성 (with enable CSM)

```
export CLUSTER_NAME=online-boutique
export REGION=us-central1
```

```
gcloud container clusters get-credentials ${CLUSTER_NAME} --region ${REGION}
```

# [install istio-gateway](https://cloud.google.com/service-mesh/docs/operate-and-maintain/gateways#deploy_sample_gateway)

```
export GATEWAY_NAMESPACE=gateway
```

```
kubectl create namespace ${GATEWAY_NAMESPACE}
kubectl label namespace ${GATEWAY_NAMESPACE} \
    istio.io/rev- istio-injection=enabled --overwrite
```

```
git clone https://github.com/GoogleCloudPlatform/anthos-service-mesh-packages
cd anthos-service-mesh-packages
```

```
kubectl apply -n ${GATEWAY_NAMESPACE} -f samples/gateways/istio-ingressgateway
```

```
$ kubectl get po,svc -n ${GATEWAY_NAMESPACE}
NAME                                        READY   STATUS    RESTARTS   AGE
pod/istio-ingressgateway-7cdd85dc66-7hp4w   1/1     Running   0          4m54s
pod/istio-ingressgateway-7cdd85dc66-kcqzd   1/1     Running   0          4m54s
pod/istio-ingressgateway-7cdd85dc66-tq8xm   1/1     Running   0          4m54s

NAME                           TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                      AGE
service/istio-ingressgateway   LoadBalancer   34.118.239.183   34.171.177.220   15021:31535/TCP,80:30966/TCP,443:32244/TCP   5m20s
```
# [microserviced-demo](https://raw.githubusercontent.com/GoogleCloudPlatform/anthos-service-mesh-samples/main/docs/shared/online-boutique/kubernetes-manifests.yaml)

```
export OB_NAMESPACE=onlineboutique
```
```
kubectl create namespace ${OB_NAMESPACE}
kubectl label namespace ${OB_NAMESPACE} \
    istio.io/rev- istio-injection=enabled --overwrite
```
```
kubectl apply -n ${OB_NAMESPACE} -f ./kubernetes
```

```
$ kubectl get po,svc -n ${OB_NAMESPACE}
NAME                                         READY   STATUS     RESTARTS       AGE
pod/adservice-5bd97d7447-r96dm               2/2     Running    0              4m27s
pod/cartservice-868845857f-zj4f6             2/2     Running    0              4m27s
pod/checkoutservice-565c8fcf44-p6klp         2/2     Running    0              4m23s
pod/currencyservice-5689f9dc6d-b2nqk         2/2     Running    0              4m8s
pod/emailservice-65449687d-l79qj             2/2     Running    1 (119s ago)   4m8s
pod/frontend-5cffdc8bd-c8gwc                 2/2     Running    0              4m8s
pod/loadgenerator-5f8697c496-5hsb6           2/2     Running    0              45m
pod/paymentservice-7b9f66dcbb-nqmf4          2/2     Running    0              4m3s
pod/productcatalogservice-7fc9bdbb5b-wgpdr   2/2     Running    0              3m58s
pod/recommendationservice-77f6867c49-cfxhn   2/2     Running    1 (111s ago)   3m54s
pod/redis-cart-7f46cddc98-z7tn9              2/2     Running    0              3m52s
pod/shippingservice-7446fc579b-57tk2         2/2     Running    0              3m40s

NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
service/adservice               ClusterIP      34.118.231.220   <none>         9555/TCP       54m
service/cartservice             ClusterIP      34.118.234.87    <none>         7070/TCP       54m
service/checkoutservice         ClusterIP      34.118.229.75    <none>         5050/TCP       53m
service/currencyservice         ClusterIP      34.118.229.245   <none>         7000/TCP       53m
service/emailservice            ClusterIP      34.118.236.46    <none>         5000/TCP       53m
service/frontend                ClusterIP      34.118.235.121   <none>         80/TCP         53m
service/frontend-external       LoadBalancer   34.118.233.55    34.58.179.15   80:30390/TCP   4m13s
service/paymentservice          ClusterIP      34.118.228.195   <none>         50051/TCP      53m
service/productcatalogservice   ClusterIP      34.118.234.73    <none>         3550/TCP       53m
service/recommendationservice   ClusterIP      34.118.226.119   <none>         8080/TCP       53m
service/redis-cart              ClusterIP      34.118.225.162   <none>         6379/TCP       53m
service/shippingservice         ClusterIP      34.118.233.73    <none>         50051/TCP      53m
```

# Istio Gateway
```
kubectl apply -n ${OB_NAMESPACE} -f ./istio-gateway
```
```
$ kubectl get svc -n ${GATEWAY_NAMESPACE} 
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                      AGE
istio-ingressgateway   LoadBalancer   34.118.239.183   34.171.177.220   15021:31535/TCP,80:30966/TCP,443:32244/TCP   37m
```
```
34.171.177.220
```

# Gateway API
```
kubectl apply -n ${OB_NAMESPACE} -f ./k8s-gateway-api
```

# [Istio - Traffic Shifting]()


