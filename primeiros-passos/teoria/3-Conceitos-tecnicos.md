# Conceitos técnicos

## Pod Kubernetes

Um pod do Kubernetes é um conjunto de um ou mais containers Linux®, sendo a menor unidade de uma aplicação Kubernetes. Os pods são compostos por um container nos casos de uso mais comuns ou por vários containers fortemente acoplados em cenários mais avançados. Os containers são agrupados nesses pods para que os recursos sejam compartilhados de modo mais inteligente, como descrito abaixo.

## ReplicationController e Replicasets

- O Controller Manager é o cérebro por trás de toda a automação do Kubernetes.
- Este componente monitora os objetos presentes no cluster e age de acordo com as necessidades.
- Uma das ferramentas que faz parte do Controller Manager é o Replication Controller.
- O Replication Controller nos ajuda a executar múltiplas instâncias de um pod/container no nosso cluster Kubernetes.
- O que ele faz é se certificar que o número especificado de pods do cluster estejam sempre sendo executados.
- Outro fator importante para o uso do Replication Controller é a realização de balanceamento de carga e escalamento da nosso aplicação.

 ![alt text](/teoria/images/replication-controller-and-replicaset.png)

- Replication Controller é uma ferramenta "mais antiga" que está sendo substituida pelo Replicaset.

- No Replicaset precisamos definir o "selector".

- Para que servem as labels e selector?
  - Para identificação dos pods (imagine um cenário com diversos pods sendo executados).

## Deployment

- A ideia do deployment é fazermos a publicação da aplicação.
- Imagine que temos uma aplicação web e precisamos fazer o deployment dessa aplicação. Então fazemos o deployment com vários containers para dividir a nossa carga de trabalho. Depois de um tempo fizemos melhorias e temos uma versão 2 da nossa aplicação. E como fazemos para publicar? Geralmente podemos pensar que iremos simplesmente realizar a publicação da nova versão em todos os pods de uma só vez. No entanto, isso pode impactar os usuários que estão acessando a aplicação. Assim, o que queremos fazer é atualizar um por um até que todos tenham sido atualizados. Isso é conhecido como "rolling release / rolling updates". Suponha que um dos pods apresentou um problema inesperado, e decidimos reverter a situação para analisar. Então devemos executar um "rollback".
- Para fazer tudo isso, o kubernetes tem a ferramenta de Deployment.

![alt text](/teoria/images/deployment.png)

### Desfazendo e Atualizando Deployments

- Durante a atualização de aplicações podem ocorrer problemas, e por isso é importante conseguir desfazer atualizações (rollback).
- Assim, quando realizamos um deploy com o Deployment do Kubernetes, é disparado um recurso chamado "rollout".
- A cada execução do rollout é criado uma revisão de publicação. Este processo ajuda o Kubernetes a manter o rastro de publicações da aplicação, fazendo com que facilmente possa ser realizado um "rollback" para qualquer revisão anterior em caso de problemas.

### Estratégias de publicação

- Recreate:
  - Todas as instâncias são desligadas (shutdown).
  - E novas instâncias são criadas.
  - O problema é que o cliente ficara sem acesso a aplicação.

- Rolling Update:
  - Atualiza uma por uma de cada vez.
  - Mantem a aplicação sempre disponível (alta disponibilidade).
  - Estratégia padrão usada pelo Kubernetes.

### Rollback

- Quando temos problemas com a atualização, damos um rollback.
- Ele irá destruir as instâncias com a atualização e criar novamente instâncias com a versão anterior.
- Tudo de acordo com a estratégia de atualização.

