# helm

```mac
brew install helm
helm version
```

[helm :: rabbitmq](https://github.com/bitnami/charts/blob/main/bitnami/rabbitmq/values.yaml)

```helm
 helm list
 helm repo add bitnami https://charts.bitnami.com/bitnami
 helm repo update
 helm install -f rb-values.yaml rabbitmq bitnami/rabbitmq
 kubectl get all
```

```clean
helm list
helm delete rabbitmq
helm repo remove bitnami 
```

`helm install -f rb-values.yaml [name] [repository/name]`
