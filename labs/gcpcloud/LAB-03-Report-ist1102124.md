# Q1
Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/google...
- Installing hashicorp/google v3.89.0...
- Installed hashicorp/google v3.89.0 (signed by HashiCorp)

# Q2
O terraform-gcp-servers.tf trata da criação de instancias na GCP. Cria X (count variable) instancias de Web Servers identicas e cria um Load Balancer. É especificado o sistema operativo, a rede e as ssh-keys.

# Q3
O terraform-gcp-variables.tf trata da configuração das variaveis correspondentes ao GCP. É especificado o Project ID no qual vamos dar deploy das maquinas, a regiao do Server GCP e o tipo e tamanho default do disco de cada maquina.

# Q4
Para criar mais Web Servers basta mudar a variavel count linha 10 do ficheiro terraform-gcp-servers.tf.

# Q5
Aparecereu o ficheiro terraform.tfstate e o terraform.tfstate.backup.

# Q6
No ficheiro gcphosts mudei os ips do balancer e dos web servers pelos ips que estavam na cloud. O objetivo foi para o mgmt node conseguir aceder aos web servers e ao balancer da cloud.

# Q7
web1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web3 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
balancer | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

Foi bem sucedido porque o ficheiro ansible.cfg tem como inventory o ficheiro gcphosts, que por sua vez, tem os ips das maquinas da cloud definidos aquando da configuração da infraestutura.

# Q8
Linha 59 e 116 ansible-gcp-servers-setup-all.yml

# Q9
O nome do servidor e o respetivo IP mudaram. Isto acontece devido a Load Balancer nos redirecionar para outro server com o objetivo de equilibrar e evitar sobrecarga no tráfego por server.

# Q10
google_compute_instance.balancer: Destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/balancer]
google_compute_instance.web[2]: Destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3]
google_compute_instance.web[0]: Destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1]
google_compute_instance.web[1]: Destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web2]
google_compute_firewall.frontend_rules: Destroying... [id=projects/agisit-2021-teste/global/firewalls/frontend]
google_compute_instance.balancer: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/balancer, 10s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 10s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 10s elapsed]
google_compute_instance.web[1]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web2, 10s elapsed]
google_compute_firewall.frontend_rules: Still destroying... [id=projects/agisit-2021-teste/global/firewalls/frontend, 10s elapsed]
google_compute_firewall.frontend_rules: Destruction complete after 11s
google_compute_instance.balancer: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/balancer, 20s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 20s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 20s elapsed]
google_compute_instance.web[1]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web2, 20s elapsed]
google_compute_instance.balancer: Destruction complete after 22s
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 30s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 30s elapsed]
google_compute_instance.web[1]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web2, 30s elapsed]
google_compute_instance.web[1]: Destruction complete after 32s
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 40s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 40s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 50s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 50s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m0s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 1m0s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m10s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 1m10s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m20s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 1m20s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m30s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 1m30s elapsed]
google_compute_instance.web[0]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web1, 1m40s elapsed]
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m40s elapsed]
google_compute_instance.web[0]: Destruction complete after 1m43s
google_compute_instance.web[2]: Still destroying... [id=projects/agisit-2021-teste/zones/europe-west3-a/instances/web3, 1m50s elapsed]
google_compute_instance.web[2]: Destruction complete after 1m54s

Destroy complete! Resources: 5 destroyed.

Na Tab ACTIVITY do Dashboard aparecem entradas que indicam que as VMs foram apagadas.