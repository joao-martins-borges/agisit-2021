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
    The terraform-vmcloud-servers.tf is the file that oversees the creation of virtual machine instances in the cloud service, in this case VMCloud. It has a section dedicated to the creation of the web servers (count equals the number of web servers to be created), and another section dedicated to the creation of the Load Balancer instance in the cloud service. It also has an initial instance dedicated to the creation of a name for the keypair which is used in the creation of the Web servers and Load Balancer instances, to avoid conflicts in the project.

# Q03
    The terraform-vmcloud-networks.tf file specifies the protocols to be used in order to access the cloud servers, in this case HTTP and HTTPS.
# Q04
    To create more Web servers we need to access the terraform-vmcloud-servers.tf and go to the web servers section. There, in line 27, we need just alter the count value to the number of web servers we want to be deployed within the cloud service.
# Q05
    After running the terraform apply command, two files are created in the vmcloud directory, being: terraform.tfstate and terraform.tfstate.backup
# Q06
    After creating the infrastructure with Terraform, we updated the myhosts file in lines 6 to 9. In these lines we updated the X characters with the IP adress corresponding to each virtual machine that was deployed in the VMCloud, in order to allow the communication between our local virtual machine (management node) and the mahcines deployed in the VMCloud (Web servers and Load Balancer).

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

    The command was successful. That is because after we created our infrastructure with Terraform, we deployed our virtual machines in the VMCloud. Right after, we updated the IP adresses of these recently created machines in the myhosts file. SInce this file is the inventory file for Ansible, Ansible had all the information needed to connect to the virtual machine instances in the cloud service.
# Q08
    In the default-site-j2 file, I deleted line 6, more specifically “listen 443 ssl;”.
    In the index-html.j2 file, I changed line 19 to "hostvars[ansible_hostname]['ansible_host']", in order to fetch the IP adress of the web server machines in the cloud service.
    In the haproxy-cfg.j2 file, I changed line 56 to hostvars[host]['ansible_default_ipv4']['address'], in order to access the hosts' IP to use in the server variable.
# Q09
    When the system was deployed, when hitting the refresh button (forcing with the Shift key), we would cycle through the connection of the Load Balancer with each of the running web server virtual machines, changing the IP address that was displaying to the corresponding IP of the web server in the cloud service, and the hostname of the corresponding machine. 
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
    openstack_compute_secgroup_v2.sec_ingr: Destruction complete after 2s

    Destroy complete! Resources: 6 destroyed.

    After the destroy, in the dashboard, I noticed that the security groups that were created, have been destroyed upon the terraform destroy command. However, the network and routers maintained the same since they are not a part of the virtual machines, but the ports came from 7 active ports to 4. Regarding the volume and compute, both were changed (decreased since the machines were destroyed). In conclusion, every resource created in the deployment of the machines is destroyed after the terraform destroy command.