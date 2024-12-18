# Arquivos de definição

- Um arquivo de definição Kubernetes é escrito na linguagem YAML.
- Sempre tem 4 top level (nível raíz):

```bash
# Neste campo definimos a versão do objeto que estamos criando na API Kubernetes.
# Nem todo objeto é necessário definir uma versão para a API.
apiVersion:

# Define o tipo de objeto do Kubernetes (Pod, Service, Replicaset, Deployment, ...)
kind:

# São dados pertencentes a um determinado objeto.
# So podemos definir valores para campos de metadada reconhecidos pelo Kubernetes.
metadata:

# É referente as especificações do objeto Kubernetes que está sendo criado (template).
spec:
```

