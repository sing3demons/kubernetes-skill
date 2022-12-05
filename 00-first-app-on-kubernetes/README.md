# 00-first-app-on-kubernetes

## deploy

<hr>

```touch
touch 01-nginx-deploy.yaml
```

```nginx-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx # template.metadata.labels.app: nginx
  template: # pod
    metadata:
      labels:
        app: nginx # <- matchLabels.app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80

```

```kube cmd
kubectl apply -f 01-nginx-deploy.yaml
kubectl get deploy
kubectl get pod
kubectl exec -it nginx-deployment-6595874d85-5q2z5 -- sh
```

exec kube -> `kubectl exec -it [NAME] -- sh`

dev/devops -> kubectl -> kube-apiserver -> deployment -> replicaset -> Pod

<hr>

## service

หน้าที่ของ service คือ เปิด connection ให้ user ภายนอกเข้ามาใช้บริการได้

### Service type

- ClusterIP ใช้คู่กับ ingress-nginx
- NodePort ใช้คู่กับ spec.ports.nodePort (default: 30000-32767).
- LoadBalancer # แพง

### Port Port Port

- port service ภายใน kubernetes เรียกกันเอง ให้ใช้ port ภายใน
- targetPort จะต้องตรงกับ container port ภายนอก
- nodePort

```touch
touch 02-nginx-service.yaml
```

```vi 02-nginx-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx # kind: Deployment :: spec.template.metadata.labels.app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30180
```

```kube cmd
kubectl apply -f 02-nginx-service.yaml
kubectl get svc
kubectl describe service nginx-service
```

http://localhost:30180/

 <hr>
 ## Ingress

```touch
touch 03-nginx-service-ingress.yaml
```

```Docker Desktop
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/aws/deploy.yaml
kubectl get ns
kubectl get all -n ingress-nginx
kubectl apply -f 03-nginx-service-ingress.yaml
kubectl get ing
kubectl describe ingress minimal-ingress
kubectl get po -o wide
```

```get
kubectl get svc -n ingress-nginx
```

http://localhost/testpath

<hr>

## ConfigMap

```clean up
kubectl delete -f 01-nginx-deploy.yaml
kubectl delete -f 02-nginx-service.yaml
kubectl delete -f 03-nginx-service-ingress.yaml
```

or `kubectl delete -f . `

```
kubectl apply -f 00-nginx-configmap.yaml
kubectl apply -f 00-nginx-deploy-config.yaml
```

<hr>
