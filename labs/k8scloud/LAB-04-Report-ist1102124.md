# Q1
vagrant@mgmt:~/labs/k8scloud$ terraform init
    Initializing modules...
    - gcp_gke in gcp_gke
    - gcp_k8s in gcp_k8s

    Initializing the backend...

    Initializing provider plugins...
    - Finding latest version of hashicorp/google...
    - Finding latest version of hashicorp/kubernetes...
    - Installing hashicorp/google v3.90.0...
    - Installed hashicorp/google v3.90.0 (signed by HashiCorp)
    - Installing hashicorp/kubernetes v2.6.1...
    - Installed hashicorp/kubernetes v2.6.1 (signed by HashiCorp)

# Q2
O terraform.tfvars inicializa as variaveis do projeto GCP necessárias para dar deploy das VMs através do terraform. É especificado o ID do projeto, o umero de worker nodes do Kubernetes cluster e a região do GCP.

# Q3
O gcp-gke-main.tf especifica as configurações para a criação do Kubernetes cluster. São usadas as variaveis do ficheiro terraform.tfvars para criar o cluster corretamente.

# Q4
Na linha 11 do ficheiro terraform.tfvars.

# Q5
Não obtive nenhum erro. Penso que isso tenha acontecido porque foi escolhida a região GCP com recursos dispoiveis para o cluster.

# Q6
Foram criados o terraform.tfstate e o terraform.tfstate.backup.

# Q7
Ao analisar o gcp-gke-main.tf as variaveis sao obtidas a partir do terraform.tfvars e as variaveis do k8s-provider.tf são obtidas após o Provisioning do cluster. Se abrirmos o gcp-gke-cluster.tf conseguimos ver as output variables (linhas 54-74).

# Q8
A ordem das declarations importa porque o Frontend Pods necessitaque o Backend Pods tenha sido deployed para conseguir acessar e usar os seus serviços.

# Q9
    vagrant@mgmt:~/labs/k8scloud$ kubectl get service  
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend         LoadBalancer   10.123.240.79    35.246.190.79   80:31474/TCP   14m
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        15m
    redis-follower   ClusterIP      10.123.255.158   <none>          6379/TCP       14m
    redis-leader     ClusterIP      10.123.240.142   <none>          6379/TCP       14m

# Q10
    vagrant@mgmt:~/labs/k8scloud$ kubectl get pods
    NAME                              READY   STATUS    RESTARTS   AGE
    frontend-555584b8c9-bp82k         1/1     Running   0          13m
    frontend-555584b8c9-dqxhl         1/1     Running   0          13m
    frontend-555584b8c9-xmg6x         1/1     Running   0          13m
    redis-follower-6579bcb987-bcjdq   1/1     Running   0          13m
    redis-follower-6579bcb987-p249n   1/1     Running   0          13m
    redis-leader-769c885c4f-6r4vn     1/1     Running   0          13m

    ___________________________________________________________________________________

    vagrant@mgmt:~/labs/k8scloud$ kubectl get service  
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend         LoadBalancer   10.123.240.79    35.246.190.79   80:31474/TCP   14m
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        15m
    redis-follower   ClusterIP      10.123.255.158   <none>          6379/TCP       14m
    redis-leader     ClusterIP      10.123.240.142   <none>          6379/TCP       14m
