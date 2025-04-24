# GKE_Statefulset
Migração de uma Aplicação usando StatefulSet + NodeAffinity

## PROCESSOS:
Obs: Imaginamos que sua aplicação esteja rodando no seu cluster K8s e precisa ser migrada para um novo Node Pool com mais recursos, etc..
1- Faça o dimensionamento (Terraform - PipeLine - API provider) de um novo Node Pool.

2- Identifique os nós do novo pool com uma label exclusiva:   kubectl get nodes  //  kubectl label nodes <nome-do-node> node-pool=novo-pool  // kubectl get nodes --show-labels | grep novo-pool

3- Modifique o arquivo YAML do StatefulSet para incluir regras de afinidade

4- Aplique as alterações e depois verifique a migração:        kubectl apply -f seu-statefulset.yaml   //  kubectl get pods -o wide | grep novo-pool


## O nodeAffinity obriga o Kubernetes a agendar os pods apenas nos nós com a label especificada.
      
