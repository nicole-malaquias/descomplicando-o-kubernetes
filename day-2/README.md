## Pod

O *Pod* é o menor elemento do Kubernetes e pode conter mais de um contêiner. Todos os contêineres dentro do *Pod* compartilham a mesma rede e *namespace* (kernel). Como o *Pod* é a menor unidade do K8s, nunca haverá um contêiner em execução em um nó e outro em outro nó. Quando existem dois contêineres dentro do *Pod*, eles compartilham o mesmo endereço IP.

Para saber o conteúdo interno do *Pod*, use o seguinte comando:

```bash
kubectl describe pod {nome do pod}

```

Para pegar o conteúdo e enviar para o YAML, utilize o comando:

```bash
kubectl describe pod {nome do pod} -o yaml

```

Para listar os *Pods* com a informação de IP e *node*, execute o comando:

```bash
kubectl get pods -o wide

```

Para criar um *Pod* pela linha de comando, utilize o comando:

```bash
kubectl run {nome do pod} --image nginx --port 80

```

Para criar um *Pod* e entrar dentro, utilize o seguinte comando:

```bash
kubectl run -ti {nome do pod} --image busybox

```

Cuidado com *Pods* que podem estar com processos em execução. Para entrar dentro do container do *Pod*, utilize o seguinte comando:

```bash
kubectl attach {nome do pod} -c {nome do container do pod} -ti

```

Caso o *Pod* seja um *nginx* que o processo está executando o tempo todo, utilize o seguinte comando:

```bash
kubectl exec -it {nome do pod} -c {nome do pod} -- /bin/sh

```

A diferença entre `create` (cria) ou `apply` (cria se não existir ou atualiza) é que o `apply` pode atualizar um *pod* já existente sem precisar criá-lo novamente. Para utilizar o `apply`, execute o comando:

```bash
kubectl apply -f pod.yaml

```

Para pegar os container e ficar observando, utilize o seguinte comando:

```bash
kubectl get pods -w

```

Para ver os logs de um container específico dentro do *Pod*, utilize o seguinte comando:

```bash
kubectl logs {nome do pod} -c {nome do container}

```

Para executar o arquivo `pod.yaml`, utilize o comando:

```bash
kubectl apply -f pod.yaml

```

O arquivo `pod.yaml` deve seguir a seguinte estrutura:

```yaml
apiVersion: v1
kind: Pod # o que está sendo criado
metadata: # informações sobre o pod
  labels:
    run: girus # precisa ter sempre
    service: webserver
  name: girus # nome do pod
spec: # configurações de como o pod vai funcionar
  containers:
  - image: nginx # esse traço significa que é um container; se tivesse outro, teria outro - image ...
    name: girus
    resources: {} # se tem alguma limitação de recursos, exemplo: memória, CPU
  dnsPolicy: ClusterFirst # como o pod vai se comunicar com o dns
  restartPolicy: Always # teve bug e reiniciou o pod
status: {}

```

<aside>
🚨 Todo pod precisa ser criado com limite de uso de memoria.

</aside>

### Adicionando volume ao pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels: 
    run: giropops-empytdir
  name: giropops-empytdir
spec: 
  containers:
  - image: nginx
    name: webserver
    volumeMounts:
    - mountPath: /giropops
      name: primeiro-emptydir
    resources: 
      limits: 
        cpu: "1.0"
        memory: "128Mi"
      requests:
        cpu: "0.5"
        memory: "64Mi"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:  
  - name: primeiro-emptydir  # precisa ser o mesmo nome do volume do container
    emptyDir: 
      sizeLimit: "256Mi"
```

<aside>
🚨 Se o pod for movido para outro local, todos os dados serão perdidos. No entanto, se o pod for reiniciado, o diretório com os dados permanecerá intacto.
</aside>