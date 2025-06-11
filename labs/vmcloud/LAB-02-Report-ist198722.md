# Q1
Initializing provider plugins...
- Finding latest version of hashicorp/random...
- Finding latest version of terraform-provider-openstack/openstack...
- Installing hashicorp/random v3.1.0...
- Installed hashicorp/random v3.1.0 (signed by HashiCorp)
- Installing terraform-provider-openstack/openstack v1.44.0...
- Installed terraform-provider-openstack/openstack v1.44.0 (self-signed, key ID 4F80527A391BEFD2)

# Q2
O ficheiro "terraform-vmcloud-servers.tf" serve para criar as instancias do loadbalancer e do webserver (neste caso 2 webservers) já com o nome,imagem,o tipo da maquina,key_pair e security group configurados. E também cria um nome aleatório para evitar conflitos no projeto.

# Q3
O ficheiro "terraform-vmcloud-networks.tf" cria um security_group com as regras necessárias para acessar os servidores via HTTP e HTTPS.

# Q4
Para criar mais web servers basta alterar o ficheiro "terraform-vmcloud-servers.tf" na linha 27 para "count=x" sendo x o número desejado de máquinas.

# Q5
Os ficheiros criados após o comando "terraform apply" são o "terraform.tfstate" e "terraform.tfstate.backup"

# Q6
O ficheiro "myhost" foi modificado para configurar os ips do balancer e do webserver para eventualmente ser usado pelo ansible.

# Q7
Como já tinha definido os ips das instancias no "myhost" e sendo este usado como o inventory do ansible foi possivel fazer o ansible ping para todos os targets desse ficheiro.

balancer | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
web1 | SUCCESS => {
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

# Q8
haproxy.j2 linha 59 para "server {{ host }} {{ hostvars[host]['ansible_default_ipv4']['address'] }}:80 check"
index.html.j2 linha 19 para "<h1>({{ hostvars[ansible_hostname]['ansible_host'] }})</h1>"
default.j2 apaguei a linha 6 que dava listen ao ssl e por isso pedia um certeficado valido.

# Q9
Quando se fazia o refresh da página o server que estavamos a aceder mudava entre web1 e web2 e os respectivos ips da vmcloud isto porque o loadbalancer escolhia os servidores que estavamos a aceder para diminuir a carga de requests para cada webserver.

# Q10
Ainda restam alguns recursos referentes à network no entanto todos os recursos criados pelo apply foram destruidos.

openstack_compute_instance_v2.web[0]: Destroying... [id=71b24c46-bb9a-4f24-86ec-d01f9cfc4348]
openstack_compute_instance_v2.balancer: Destroying... [id=8587292f-b0e0-4577-9ce9-99e7809076f9]
openstack_compute_instance_v2.web[1]: Destroying... [id=a9258185-1555-4054-8897-daa343ad1a86]
openstack_compute_instance_v2.web[0]: Still destroying... [id=71b24c46-bb9a-4f24-86ec-d01f9cfc4348, 10s elapsed]
openstack_compute_instance_v2.balancer: Still destroying... [id=8587292f-b0e0-4577-9ce9-99e7809076f9, 10s elapsed]
openstack_compute_instance_v2.web[1]: Still destroying... [id=a9258185-1555-4054-8897-daa343ad1a86, 10s elapsed]
openstack_compute_instance_v2.balancer: Destruction complete after 12s
openstack_compute_instance_v2.web[1]: Destruction complete after 12s
openstack_compute_instance_v2.web[0]: Destruction complete after 12s
openstack_compute_keypair_v2.keypair: Destroying... [id=uinz]
openstack_compute_secgroup_v2.sec_ingr: Destroying... [id=36bfd064-8ad0-4cda-a390-c5733d673cb5]
openstack_compute_keypair_v2.keypair: Destruction complete after 0s
random_string.random_name: Destroying... [id=uinz]
random_string.random_name: Destruction complete after 0s
openstack_compute_secgroup_v2.sec_ingr: Destruction complete after 2s

Destroy complete! Resources: 6 destroyed.