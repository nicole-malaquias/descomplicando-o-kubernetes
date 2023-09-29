### Deployment

O deployment do Kubernetes é responsável por gerenciar a implantação de aplicativos em um cluster do Kubernetes. Ele garante que o número especificado de réplicas do aplicativo esteja em execução em todos os momentos, permitindo que o aplicativo seja escalado horizontalmente. Além disso, o deployment também é responsável por gerenciar o rollout de novas versões do aplicativo, permitindo que as atualizações sejam feitas sem tempo de inatividade.

Para buscar os pods do deployment a partir da label.

```bash
kubectl get pods -l app={nome da label}
```

Para buscar os replicasets que o deployment criou.

```bash
kubectl get replicasets 
```

Para ver os detalhes para deployment.

```bash
kubectl describe deployments
```

Para colocar o que está no deployment dentro de um yaml. 

```bash
kubectl create deployment --image nginx --replicas 3 nginx-deployment --dry-run=client -o yaml > novo-deployment.yaml
```

Criando deployment por linha de comando.

```bash
kubectl create deployment --image nginx --replicas 3 nginx-deployment
```

Para criar namespace.

```bash
kubectl create namespace giropops --dry-run=client -o yaml > namespace.yaml
```

Para buscar buscar um deployment em uma namespace que foi definida  e o mesmo para buscar os pods

```bash
kubectl get deployments.apps -n {nome da namespace}
kubectl get pods -n {nome da namespace}
```

Se houver necessidade de atualizar algum detalhe do pod, a estratégia do Kubernetes será fazê-lo em partes, mantendo a aplicação em execução e atualizando um pod de cada vez.

Para ver os detalhes do deployment que não está mais no namespace default. 

```bash
kubectl describe deployments.apps -n {nome da namespace} {nome do deployment}
```

Para executar um pod que está em um namespace especifico 

```bash
kubectl exec -ti -n {nome do namespace} {nome do pod} --bash
```

## Strategy

### RollingUpdate

Na hora de atualizar é possível especificar quantos podes serão atualizados por rodada e qual será o limite de pods dentro do node. 

```bash
...
strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1 # pode ter até 1 pode a mais no que eu pedi , se tenho 10 pods na hroa de atualizar posso chegar a 11
      maxUnavailable: 2 # vai atualizar de 2 em 2, quantidade indisponivel por update
...
```

Para saber como está o andamento de atualizações no deployment

```bash
kubectl rollout status deployment -n giropops nginx-deployment
```

Pausando o apply:

```bash
# geralmente para coisas em produção 

kubectl rollout pause deployment -n {nome da namespace} {nome do deployment}
```

Reativando o apply do ponto em que ele foi pausado: 

```bash
# fazer em ordem se não dá errado 
kubectl apply -n {nome da namespace} {nome do deployment}
kubectl rollout resume deployment -n {nome da namespace} {nome do deployment}
```

Restartando o deploy, fazendo que ele comece todo o processo matando o que estava sendo feito

```bash
kubectl rollout restart deployment -n {nome da namespace} {nome do deployment}
```

### Recreate

```bash
...
strategy:
    type: Recreate
...
```

> Faz com todos de uma vez.
> 

### Recreate x  RollingUpdate

Quando usar o recreate: Quando há mudanças muito grandes, que muda a forma da aplicação de conversar com o banco de dados que se não for da forma especifica não funciona.

Quando usar o RollingUpdate: Para quando for preciso manter a aplicação up durante o deploy, porque para isso pode ser configurado quantos pods vão vir ficar off.


### Revisão do deployment

Toda vez que é feito uma atualização do deployment é criado uma revisão como se fosse uma pipeline do github. 

```bash
kubectl rollout history deployment -n {nome do namespace} {nome do deployment} 
```

Utilizado para voltar o deploy para versão anterior. 

```bash
kubectl rollout undo deployment -n {nome do namespace} {nome do deployment} --t
```

Vendo o que teve alteração na mudança.