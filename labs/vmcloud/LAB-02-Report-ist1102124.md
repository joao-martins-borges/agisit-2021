# Q01
Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/random...
- Finding latest version of terraform-provider-openstack/openstack...
- Installing hashicorp/random v3.1.0...
- Installed hashicorp/random v3.1.0 (signed by HashiCorp)
- Installing terraform-provider-openstack/openstack v1.44.0...
- Installed terraform-provider-openstack/openstack v1.44.0 (self-signed, key ID 4F80527A391BEFD2)


# Q02
O terraform-vmcloud-servers.tf cria keypairs com nomes aleatórios para evitar conflitos, cria X instancias de Web Servers identicas usando os keypairs e cria ainda um Load Balancer.

# Q03
O terraform-vmcloud-networks.tf especifica os protocolos usados para aceder aos web severs (HTTPS e HTTP).

# Q04
Para criar mais Web Servers basta mudar a variavel count linha 27 do ficheiro terraform-vmcloud-servers.tf.

# Q05
Aparecereu o ficheiro terraform.tfstate e oterraform.tfstate.backup.

# Q06
No ficheiro myhosts mudei os ips do balancer e dos web servers pelos ips que estavam na cloud. O objetivo foi para o mgmt node conseguir aceder aos web servers e ao balancer da cloud.

# Q07
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

Foi bem sucedido porque o ficheiro ansible.cfg tem como inventory o ficheiro myhosts, que por sua vez, tem os ips das maquinas da cloud definidos aquando da configuração da infraestutura.


# Q08
Apaguei a linha 6 do ficheiro default-site.j2, mudei no index.html.j2 a linha 19 a variavel utilizada por hostvars[ansible_hostname]['ansible_host'] e no ficheiro haproxy.cfg.j2 a segunda variavel por hostvars[host]['ansible_default_ipv4']['address']. 

# Q09
O nome do servidor e o respetivo IP mudaram. Isto acontece devido a Load Balancer nos redirecionar para outro server com o objetivo de equilibrar e evitar sobrecarga no tráfego por server.

# Q10
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
openstack_compute_secgroup_v2.secgr: Destruction complete after 2s

Destroy complete! Resources: 6 destroyed.

No Dashboard existem ainda alguns recursos referentes à network. No entanto, todos os recursos que foram criados aquando do deployment (terraform apply) são destruidos após a execução do comando terraform destroy.