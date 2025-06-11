##  Deploy a Cloud-Native App in GCP Kubernetes Cluster

# Q1
```
    Initializing provider plugins...
    - Finding latest version of hashicorp/google...
    - Finding latest version of hashicorp/kubernetes...
    - Installing hashicorp/google v3.90.0...
    - Installed hashicorp/google v3.90.0 (signed by HashiCorp)
    - Installing hashicorp/kubernetes v2.6.1...
    - Installed hashicorp/kubernetes v2.6.1 (signed by HashiCorp)
```

# Q2
O ficheiro "terraform.tfvars" é responsável por configurar as variaveis do projeto que vão ser usadas. Neste ficheiro são definidos o project_ID, workers_count e a região das máquinas.

# Q3
O ficheiro "gcp-gke-main.tf" especifica as configurações do cluster a ser criado em que usa as variaveis especificadas no ficheiro "terraform.tfvars".

# Q4
Basta alterar a linha 11 do ficheiro terraform.tfvars para "workers_count=x" sendo x o número desejado.

# Q5
Eu não tive nenhum erro sobre o Quota limits.

# Q6
Os ficheiros criados após o comando "terraform apply" são o "terraform.tfstate" e "terraform.tfstate.backup"

# Q7
Essas variaveis são obtidas depois de fazer o provisioning do module "gcp_gke".E após analisar o ficheiro "gcp-gke-cluster.tf" observei que essas variaveis são obtidas após o provisioning do cluster.E as variaveis obtidas do ficheiro "gcp-gke-main.tf" como "project", "region", "workers_count" são obtidas atraves do terraform.tfvars.

# Q8
Sim acho que a ordem neste caso é importante, pois o frontend necessita do backend para ser totalmente deployed e para obter toda a informação necessaria para usar os serviços.
# Q9
Os ips podem ser obtidos através do proprio GCP ao ver o GKE, através do terraform.tfstate ou então através do comando abaixo. 
```
    vagrant@mgmt:~/labs/k8scloud$ kubectl get service  
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend         LoadBalancer   10.123.240.86    35.246.166.41   80:31474/TCP   6m13s
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        6m14s
    redis-follower   ClusterIP      10.123.255.147   <none>          6379/TCP       6m13s
    redis-leader     ClusterIP      10.123.240.163   <none>          6379/TCP       6m13s
```

# Q10
Após executar o comando mencionado a vm mgmt vai ligar a cloud ao kubernetes e configar o ficheiro kubeconfig.

```
    vagrant@mgmt:~/labs/k8scloud$ kubectl get service  
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend         LoadBalancer   10.123.240.86    35.246.166.41   80:31474/TCP   6m13s
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        6m14s
    redis-follower   ClusterIP      10.123.255.147   <none>          6379/TCP       6m13s
    redis-leader     ClusterIP      10.123.240.163   <none>          6379/TCP       6m13s
 ```

```
vagrant@mgmt:~/labs/k8scloud$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
frontend-555584b8c9-2mp68         1/1     Running   0          5m28s
frontend-555584b8c9-wjxfn         1/1     Running   0          5m29s
frontend-555584b8c9-z2rgt         1/1     Running   0          5m28s
redis-follower-6579bcb987-5ns8q   1/1     Running   0          5m28s
redis-follower-6579bcb987-v8ng6   1/1     Running   0          5m28s
redis-leader-769c885c4f-m92d7     1/1     Running   0          5m29s
```
