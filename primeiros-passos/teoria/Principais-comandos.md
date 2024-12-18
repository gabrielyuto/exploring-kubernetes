# Comandos Kubernetes

## Relacionados ao Minikube

```bash
# Verificar status
minikube status

# Iniciar cluster
minikube start

# Deletar cluster
minikube delete
```

## Trabalhando com Kubectl
```bash
# Criar um pod nginx
# Tambem cria um replicaset
kubectl run <name-pod> --image <image>

# Listar pods
kubectl get pods

# Listar pods com mais detalhes
kubectl get pods -o wide

# Descrever um pod
kubectl describe pod <name-pod>

# Criar um pod a partir de um arquivo.yml
kubectl create -f <arquivo.yml> --save-config

# Tambem podemos usar esse comando para criar
kubectl apply -f <arquivo.yml>

# Deletar um pod
kubectl delete pod <name-pod>

# Listar replicaset
kubectl get replicaset

# --- PARA ESCALAR UMA APLICAÇÃO --- 
# Para atualizar alguma mudança que foi feita num arquivo de definicao
kubectl replace -f <arquivo.yml>

# Para alterar um parametro especifico
kubectl scale --replicas=6 -f <arquivo.yml>

# Também podemos utilizar o seguinte comando
kubectl scale --replicas=8 replicaset <nome do objeto>

# Podemos tambem descrever um replicaset
kubectl describe replicaset <nome replicaset>

# --- DEPLOYMENT ---
# Consultar os deployments
kubectl get deployment

# Para consultar o status do rollout
kubectl rollout status deployment/<nome-deployment>

# Para consultar o historico de rollout
kubectl rollout history deployment/<nome-deployment>

# Podemos desfazer o rollout, dando um rollback
kubectl rollout undo deployment/<nome-deployment>
```
