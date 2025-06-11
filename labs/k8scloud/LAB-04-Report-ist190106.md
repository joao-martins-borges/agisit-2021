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
    The terraform.tfvars file is responsible for the initialization of the GCP project variables needed for terraform to deploy the virtual machines. In this file, we specified the ID of the project, how many worker nodes we wanted in the Kubernetes cluster and the GCP region we want them to be deployed in.
# Q3
    The gcp-gke-main.tf file is where we specify the configurations for the creation of the Kubernetes cluster in and of itself, in which our worker nodes (the virtual machines) will be deployed. In this file we make use of the variables in the terraform.tfvars, as mentioned in the previous question, to correctly create the cluster.
    Example:
        module "gcp_gke" {
        source   = "./gcp_gke"
        project = var.project
        region = var.region
        workers_count = var.workers_count
        }
# Q4
    If we wanted to create a smaller/larger cluster we just need to change the variable "workers_count", in line 11 of terraform.tfvars file, to our desired number of nodes (virtual machines) that we want to deploy within the cluster. In our case, we changed this value to '2', and 2 nodes were deployed in our Kubernetes cluster.
# Q5
    We had no errors related to Quota limits. We think that this was the case because we chose a GCP region with available resources for our cluster and deployment.
# Q6
    After running the terraform apply command, the files that were created were: terraform.tfstate and terraform.tfstate.backup.
# Q7
    The variables present in the gcp-gke-main.tf file are initialized in the terraform.tfvars file. The variables present in the k8s-provider.tf are only obtained after the provisioning of the cluster in GCP is done. As we can the, the output of this provisioning is in the file "gcp-gke-cluster.tf" and the variables present in this file are the ones that are used in the k8s-provider.tf file.
# Q8
    The specifications of the Backend Pods makes sense to come before the Frontend Pods because the Frontend Pods may need to access the services and use the information provided by the Backend Pods, so they need to be specified beforehand. In the Google tutorial 'Create a guestbook with Redis and PHP' this is also the same specification order they use, meaning Backend before Frontend.
# Q9
    vagrant@mgmt:~/labs/k8scloud$ kubectl get service  
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend         LoadBalancer   10.123.240.84    35.246.208.86   80:31474/TCP   14m
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        15m
    redis-follower   ClusterIP      10.123.255.171   <none>          6379/TCP       14m
    redis-leader     ClusterIP      10.123.240.155   <none>          6379/TCP       14m

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
    frontend         LoadBalancer   10.123.240.84    35.246.208.86   80:31474/TCP   14m
    kubernetes       ClusterIP      10.123.240.1     <none>          443/TCP        15m
    redis-follower   ClusterIP      10.123.255.171   <none>          6379/TCP       14m
    redis-leader     ClusterIP      10.123.240.155   <none>          6379/TCP       14m

    ___________________________________________________________________________________

    vagrant@mgmt:~/labs/k8scloud$ gcloud container clusters describe guestbook --region europe-west3-a --format='default(locations)'
    Fetching cluster endpoint and auth data.
    kubeconfig entry generated for guestbook.
    ___________________________________________________________________________________

    The gcloud container command links our Kubernetes application with the cloud service. As we can see, it seeks our cluster endpoint to link the guestbook to.