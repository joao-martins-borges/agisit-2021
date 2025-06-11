# Q01
    Initializing the backend...

    Initializing provider plugins...
    - Finding latest version of hashicorp/google...
    - Installing hashicorp/google v3.89.0...
    - Installed hashicorp/google v3.89.0 (signed by HashiCorp)

    Terraform has created a lock file .terraform.lock.hcl to record the provider
    selections it made above. Include this file in your version control repository
    so that Terraform can guarantee to make the same selections by default when
    you run "terraform init" in the future.

    Terraform has been successfully initialized!

    You may now begin working with Terraform. Try running "terraform plan" to see
    any changes that are required for your infrastructure. All Terraform commands
    should now work.

    If you ever set or change modules or backend configuration for Terraform,
    rerun this command to reinitialize your working directory. If you forget, other
    commands will detect it and remind you to do so if necessary.
# Q02
    The terraform-gcp-servers.tf file is in charge of overseeing the creation of virtual machine instances in the Google Cloud Platform service. This file has a section dedicated to the creation and configuration of the web servers (the count variable instances how many will be created), and the creation and configuration of the Load Balancer machine. In these configurations, it is specified the operating system, network and ssh-keys for the virtual machines.
# Q03
    The terraform-gcp-variables.tf file is in charge of overseeing the configuration of variables regarding the corresponding project inside the Google Cloud Platform service and overall machine specifications. In this file we specify the corresponding Project ID of where we want to deploy the machines, the GCP server region where the machines will be available, the machine type and the default disk size for each.
# Q04
    If we needed more Web Servers, we only needed to change the variable named 'count', in line 10 of the terraform-gcp-servers.tf file, to the required number of Web servers that we need. What we need to change next is only for Ansible, in the inventory file.
# Q05
    After running the terraform apply command, two files are created in the vmcloud directory, being: terraform.tfstate and terraform.tfstate.backup
# Q06
    After creating the infrastructure with Terraform, we updated the gcphosts file in lines 6 to 9. In these kines we updated the 'X' characters with the IP address corresponding to each virtual machine that was deployed in the Google Cloud Platform, in order to allow the communication between our local virtual machine (management node) and the machines deployed in the GCP (Web servers and Load Balancer).
# Q07
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

    The command was successful. That is because after we created our infrastructure with Terraform, we deployed our virtual machines in the Google Cloud Platform. Right after, we updated the IP adresses of these recently created machines in the gcphosts file. Since this file is the inventory file for Ansible, Ansible had all the tools needed to connect to the virtual machine instances deployed in GCP.
# Q08
    The files we needed to modify were:
    - ansible-gcp-servers-setup-all.yml: we changed the paths in lines 59 and 116 to the correct paths of the haproxy.cfg.j2 and index.html.j2 files.
    - ansible.cfg: we changed the path in line 5 because the path that was specified was not the correct path to the ansible inventory file (gcphosts).
# Q09
    When the system was deployed, when hitting the refresh button (forcing with the Shift key), we would cycle through the connection of the Load Balancer with each of the running web server virtual machines, changing the IP address that was displaying to the corresponding IP of the web server in the Google Cloud Platform, and the hostname of the corresponding machine. In short, hitting refresh, cycled through Web 1, Web 2 and Web 3 machines and their corresponding IP, throught the load balancer.
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

    After running the terraform destroy command, in the GCP Activity tab appeared 5 new notifications regarding the 'destruction' of each virtual machine from the Google Cloud Platform environment.