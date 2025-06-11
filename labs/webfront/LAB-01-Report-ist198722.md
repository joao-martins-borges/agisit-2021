# Q1
As intruções do vagrantfile são para criar as maquinas as mgmt,balancer e até x webservers já com o nome e ip corretos e outras configurações, para além disso também mete a executar os ficheiros bootstrap.sh ,host_ip.sh e host_ssh.sh na maquina mgmt e apenas host_ssh.sh para as restantes maquinas.

# Q2
bootstrap.sh
O bootstrap.sh serve para instalar as tools necessarias para gerenciar os repositorios ppa, GNU Privacy Guard, python3 e ansible e também limpa os pacotes em cache.
host_ip.sh
Este ficheiro serve para associar os nomes das maquinas aos seus respectivos ips.
host_ssh.sh
Este ficheiro serve para apenas permitir "key based logins".

# Q3
A diferença do output do comando "ansible --version" em diretorios diferentes é o path do config file em que num diretorio qualquer é 
"config file = /etc/ansible/ansible.cfg" 
e no diretorio do projeto é 
"config file = /home/vagrant/tools/ansible.cfg".

# Q4
vagrantfile:   linha 82
inventory.ini: linha 9,20,28

# Q5
web1 | CHANGED | rc=0 >>
 21:43:53 up 44 min,  1 user,  load average: 0.01, 0.08, 0.04
web2 | CHANGED | rc=0 >>
 21:43:53 up 38 min,  1 user,  load average: 0.09, 0.06, 0.03
localhost | CHANGED | rc=0 >>
 21:43:52 up 56 min,  1 user,  load average: 0.16, 0.15, 0.07
balancer | CHANGED | rc=0 >>
 21:43:54 up 50 min,  1 user,  load average: 0.08, 0.03, 0.01
web3 | CHANGED | rc=0 >>
 21:43:53 up 18 min,  1 user,  load average: 0.06, 0.11, 0.14

 # Q6
 localhost | CHANGED | rc=0 >>
Linux mgmt 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
balancer | CHANGED | rc=0 >>
Linux balancer 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
web2 | CHANGED | rc=0 >>
Linux web2 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
web3 | CHANGED | rc=0 >>
Linux web3 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
web1 | CHANGED | rc=0 >>
Linux web1 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

# Q7
O bug acontecia porque a variavel era "server 0.europe.pool.ntp.org" e no ficheiro de ntp.conf.j2 que usa essa variavel estava "server {{ noc_ntpserver }}" o que ao ser alterado acaba por ficar 
"server server 0.europe.pool.ntp.org" por isso mudei a linha 7 do ntp.conf.j2 para apenas 
"{{ noc_ntpserver }}".


# Q8
Após executar o comando a segunda vez não houve alterações para as maquinas apenas reparei que o passo dos RUNNING HANDLER de restart do nginx nos webservers e o do haproxy no balancer não foi executado na segunda vez, isto acontece porque não houve alterações nos serviços.

# Q9
Quando se fazia o refresh da página o server que estavamos a aceder mudava entre web1,web2 e web3 isto porque o loadbalancer escolhia os servidores que estavamos a aceder para diminuir a carga de requests para cada webserver.

# Q10
Apos observar os resultados do benchmark no Status breakdown todos os request retornaram com 
codigo http 200(OK), ou seja todos os requests foram bem sucedidos.