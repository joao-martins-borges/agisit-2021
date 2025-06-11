##  Google Cloud Platform Web Service Deployment
# Q1
Initializing provider plugins...
- Finding latest version of hashicorp/google...
- Installing hashicorp/google v3.89.0...
- Installed hashicorp/google v3.89.0 (signed by HashiCorp)

# Q2
O ficheiro "terraform-gcp-servers.tf" serve para saber quais instancias é que vão ser criadas na google cloud platform  service e qual o seu nome, machine type, zone, network settings,image e as ssh keys para cada máquina. Neste caso vão ser criados 3 servidores e 1 load balancer.

# Q3
O ficheiro "terraform-gcp-variables.tf" é responsavel por configurar as variaveis do projeto que vão ser usadas. Neste ficheiro são definidos o project_ID, machine type, a região das máquinas e o espaço do disco.

# Q4
Para adicionar mais webservers basta mudar a linha 10 "count = x" do ficheiro "terraform-gcp-servers.tf" para o número desejado de webservers.

# Q5
Os ficheiros criados após o comando "terraform apply" são o "terraform.tfstate" e "terraform.tfstate.backup"

# Q6
O ficheiro "gcphosts" foi modificado para configurar os ips do balancer e dos webservers para eventualmente ser usado pelo ansible.

# Q7
Como já tinha definido os ips das instancias no "gcphosts" e sendo este usado como o inventory do ansible foi possivel fazer o ansible ping para todos os targets desse ficheiro.

<code>
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
</code>

# Q8
Alterei o ficheiro "ansible-gcp-servers-setup-all.yml" linha 59 e 116 porque ao path estava errado de "/home/vagrant/gcpcloud-tenant/..." para "/home/vagrant/labs/gcpcloud/..." e o ficheiro "ansible.cfg" linha 5 pela mesma razão, o path definido estava a apontar para o "gcpcloud-tenat" em vez do correto que é "labs/gcpcloud".

# Q9
Quando se fazia o refresh da página o server que estavamos a aceder mudava entre web1, web2 e web3 e os respectivos ips da gcpcloud isto porque o loadbalancer escolhia os servidores que estavamos a aceder para diminuir a carga de requests para cada webserver.

# Q10
Após analisar a tab Activity notei que os 5 items criados foram apagados neste caso 1 instancia do loadbalancer, 3 webserver e 1 firewall rule.

<br>

```
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
```

