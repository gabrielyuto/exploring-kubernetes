# 1. Introdução ao Kubernetes

- Documentação muito bacana sobre o Kubernetes: https://kubernetes.io/pt-br/docs/home/

## 1.1. O que é Kubernetes?

Kubernetes é um plataforma de código aberto, portável e extensiva para o gerenciamento de cargas de trabalho e serviços distribuídos em contêineres, que facilita tanto a configuração declarativa quanto a automação. Ele possui um ecossistema grande, e de rápido crescimento. Serviços, suporte, e ferramentas para Kubernetes estão amplamente disponíveis.

O Kubernetes foi criado pela Google, e o projeto posteriormente se tornou open-source.

# 2. Conceitos

## 2.1 Visão geral dos componentes de um cluster
![alt text](/teoria/images/componentes-cluster.png)

## 2.2. Componentes da camada de gerenciamento 
Os componentes da camada de gerenciamento tomam decisões globais sobre o cluster (por exemplo, alocação de Pods), bem como detectam e respondem aos eventos do cluster (por exemplo, inicialização de um novo Pod quando o campo replicas de um Deployment não está atendido).

Os componentes da camada de gerenciamento podem ser executados em qualquer máquina do cluster. Contudo, para simplificar, os scripts de configuração normalmente iniciam todos os componentes da camada de gerenciamento na mesma máquina, e contêineres com cargas de trabalho do usuário não rodam nesta máquina. Veja Construindo clusters altamente disponíveis com o kubeadm para um exemplo de configuração da camada de gerenciamento que roda em múltiplas máquinas.


### 2.2.1. kube-apiserver 
O servidor da API é um componente da camada de gerenciamento do Kubernetes que expõe a API do Kubernetes. O servidor da API é o front end para a camada de gerenciamento do Kubernetes.

A principal implementação de um servidor de API do Kubernetes é o kube-apiserver. O kube-apiserver foi projetado para ser escalonado horizontalmente — ou seja, ele pode ser escalonado com a criação de mais instâncias. Você pode executar várias instâncias do kube-apiserver e distribuir o tráfego entre essas instâncias.


### 2.2.2. etcd
Armazenamento do tipo chave-valor consistente e de alta-disponibilidade, usado como armazenamento de apoio do Kubernetes para todos os dados do cluster.

Se o seu cluster Kubernetes usa o etcd como seu armazenamento de apoio, certifique-se de ter um plano de backup para seus dados.

Você pode encontrar informações detalhadas sobre o etcd na documentação oficial.


### 2.2.3. kube-scheduler 
Componente da camada de gerenciamento que observa os Pods recém-criados e que ainda não foram atribuídos a um nó, e seleciona um nó para executá-los.

Os fatores levados em consideração para as decisões de alocação incluem: requisitos de recursos individuais e coletivos, restrições de hardware/software/política, especificações de afinidade e antiafinidade, localidade de dados, interferência entre cargas de trabalho, e prazos.


### 2.2.4. kube-controller-manager 
Componente da camada de gerenciamento que executa os processos de controlador.

Logicamente, cada controlador está em um processo separado, mas para reduzir a complexidade, eles todos são compilados num único binário e executam em um processo único.

Alguns tipos desses controladores são:

Controlador de nó: responsável por perceber e responder quando os nós caem.
Controlador de Jobs: observa os objetos Job, que representam tarefas únicas, e em seguida cria Pods para executar essas tarefas até a conclusão.
Controlador de EndpointSlice: preenche o objeto EndpointSlice (conecta os objetos Service e Pod).
Controlador de ServiceAccount: cria a ServiceAccount default para novos namespaces.

### 2.2.5. cloud-controller-manager 
Um componente da camada de gerenciamento do Kubernetes que incorpora a lógica de controle específica da nuvem. O gerenciador de controle de nuvem permite que você vincule seu cluster na API do seu provedor de nuvem, e separar os componentes que interagem com essa plataforma de nuvem a partir de componentes que apenas interagem com seu cluster.
O cloud-controller-manager executa apenas controladores que são específicos para seu provedor de nuvem. Se você estiver executando o Kubernetes em suas próprias instalações ou em um ambiente de aprendizagem dentro de seu próprio PC, o cluster não possui um gerenciador de controlador de nuvem.

Tal como acontece com o kube-controller-manager, o cloud-controller-manager combina vários ciclos de controle logicamente independentes em um binário único que você executa como um processo único. Você pode escalonar horizontalmente (executar mais de uma cópia) para melhorar o desempenho ou para auxiliar na tolerância a falhas.

Os seguintes controladores podem ter dependências de provedor de nuvem:

Controlador de nó: para verificar junto ao provedor de nuvem para determinar se um nó foi excluído da nuvem após parar de responder.
Controlador de rota: para configurar rotas na infraestrutura de nuvem subjacente.
Controlador de serviço: para criar, atualizar e excluir balanceadores de carga do provedor de nuvem.

## 2.3. Componentes do nó 
Os componentes do nó são executados em todos os nós, mantendo os Pods em execução e fornecendo o ambiente de execução do Kubernetes.


### 2.3.1. kubelet
Um agente que é executado em cada nó no cluster. Ele garante que os contêineres estejam sendo executados em um Pod.

O kubelet utiliza um conjunto de PodSpecs que são fornecidos por vários mecanismos e garante que os contêineres descritos nesses PodSpecs estejam funcionando corretamente. O kubelet não gerencia contêineres que não foram criados pelo Kubernetes.


### 2.3.2. kube-proxy 
kube-proxy é um proxy de rede executado em cada nó no seu cluster, implementando parte do conceito de serviço do Kubernetes.

kube-proxy mantém regras de rede nos nós. Estas regras de rede permitem a comunicação de rede com seus pods a partir de sessões de rede dentro ou fora de seu cluster.

kube-proxy usa a camada de filtragem de pacotes do sistema operacional se houver uma e estiver disponível. Caso contrário, o kube-proxy encaminha o tráfego ele mesmo.


### 2.3.3. Agente de execução de contêiner 
O agente de execução (runtime) de contêiner é o software responsável por executar os contêineres.

O Kubernetes suporta diversos agentes de execução de contêineres: Docker, containerd, CRI-O, e qualquer implementação do Kubernetes CRI (Container Runtime Interface).


## 2.4. Complementos (addons)
Complementos (addons) usam recursos do Kubernetes (DaemonSet, Deployment, etc) para implementar funcionalidades do cluster. Como fornecem funcionalidades em nível do cluster, recursos de complementos que necessitem ser criados dentro de um namespace pertencem ao namespace kube-system.

Alguns complementos selecionados são descritos abaixo.

### 2.4.1. DNS
Embora os outros complementos não sejam estritamente necessários, todos os clusters do Kubernetes devem ter um DNS do cluster, já que muitos exemplos dependem disso.

O DNS do cluster é um servidor DNS, além de outros servidores DNS em seu ambiente, que fornece registros DNS para serviços do Kubernetes.

Os contêineres iniciados pelo Kubernetes incluem automaticamente esse servidor DNS em suas pesquisas DNS.

### 2.4.2. Web UI (Dashboard) 
O dashboard é uma interface de usuário Web, de uso geral, para clusters do Kubernetes. Ele permite que os usuários gerenciem e solucionem problemas de aplicações em execução no cluster, bem como o próprio cluster.

### 2.4.3. Monitoramento de recursos do contêiner 
O monitoramento de recursos do contêiner registra métricas de série temporal genéricas sobre os contêineres em um banco de dados central e fornece uma interface de usuário para navegar por esses dados.


### 2.4.4. Logging a nivel do cluster 
Um mecanismo de logging a nível do cluster é responsável por guardar os logs dos contêineres em um armazenamento central de logs com uma interface para navegação/pesquisa.


