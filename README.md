# GKE_Statefulset
Migração de uma Aplicação usando StatefulSet + NodeAffinity

## PROCESSOS:
Obs: Imaginamos que sua aplicação esteja rodando no seu cluster K8s e precisa ser migrada para um novo Node Pool com mais recursos, etc..
1- Faça o dimensionamento (Terraform - PipeLine - API provider) de um novo Node Pool.

2- Identifique os nós do novo pool com uma label exclusiva:   kubectl get nodes  //  kubectl label nodes <nome-do-node> node-pool=novo-pool  // kubectl get nodes --show-labels | grep novo-pool

3- Modifique o arquivo YAML do StatefulSet para incluir regras de afinidade

4- Aplique as alterações e depois verifique a migração:        kubectl apply -f seu-statefulset.yaml   //  kubectl get pods -o wide | grep novo-pool

## O nodeAffinity obriga o Kubernetes a agendar os pods apenas nos nós com a label especificada.

## Vantagens em usar StatefulSet
1️⃣ Identidade Estável e Previsível
Cada pod recebe um nome único e fixo (ex: banco-0, banco-1).

Se um pod for recriado, mantém o mesmo nome e endereço de rede (DNS).
✅ Benefício: Ideal para sistemas que dependem de identificação consistente (ex: réplicas de bancos de dados).

2️⃣ Armazenamento Persistente Individualizado
Cada pod tem seu próprio Persistent Volume (PV), vinculado ao seu nome.

Dados não são perdidos mesmo se o pod for reiniciado ou realocado.
✅ Benefício: Garante integridade dos dados em aplicações como MySQL, MongoDB ou Kafka.

3️⃣ Ordem Controlada de Implantação e Escalonamento
Pods são criados/apagados um por um, em ordem (pod-0 → pod-1 → pod-2).

Atualizações são feitas de forma gradual (rolling updates com partition).
✅ Benefício: Evita inconsistências em sistemas distribuídos que exigem sequência (ex: eleição de líder).

4️⃣ Integração com Services Headless
Um Service headless (sem ClusterIP) permite comunicação direta e estável entre pods via DNS.

Cada pod tem um endereço único como: pod-0.meu-servico.namespace.svc.cluster.local.
✅ Benefício: Essencial para clusters de bancos de dados (ex: Cassandra, Elasticsearch).

5️⃣ Alta Disponibilidade com Affinity/Anti-Affinity
Você pode forçar pods a serem distribuídos em nós ou zonas diferentes para evitar pontos únicos de falha.
✅ Benefício: Tolerância a falhas em ambientes de produção.

6️⃣ Ideal para Aplicações com Estado (Stateful Apps)
Bancos de dados (PostgreSQL, Redis).
Filas de mensagens (Kafka, RabbitMQ).
Sistemas distribuídos (Zookeeper, ETCD).
