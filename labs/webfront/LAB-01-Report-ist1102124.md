Q01:
O Vagrantfile cria o mgmt node, o Balancer e os diferentes servers web com as devidas configurações. Executa ainda ficheiros ".sh", para o mgmt node executa o bootstrap.sh, o host_ip.sh e o host_ssh.sh, enquanto que para o Balancer e web servers executa apenas o host_ssh.sh.


Q02:
bootstrap.sh - instalar tools para gerir repositórios ppa, instalar GNU Privacy Guard, instalar Ansible.
hosts.ip.sh - configurar ficheiro hosts para rede interna.
hosts_ssh.sh - para permitir apenas logins "key based"


Q03:
O path referente ao config file muda de /etc/ansible/ansible.cfg para /home/vagrant/tools/ansible.cfg, no caso da diretoria tools. Isto deve-se ao facto de existir um ficheiro "ansible.cfg" na diretoria tools.


Q04: 
Vagrantfile - line 82 - (1..2).each do |i| -> (1..3).each do |i|
Inventory.ini - line 9, 20, 28 - retirar "#"


Q05:
web2 | CHANGED | rc=0 >>
 22:36:51 up 45 min,  1 user,  load average: 0.21, 0.12, 0.04
web1 | CHANGED | rc=0 >>
 22:36:51 up 46 min,  1 user,  load average: 0.20, 0.16, 0.07
balancer | CHANGED | rc=0 >>
 22:36:51 up 48 min,  1 user,  load average: 0.15, 0.05, 0.01
web3 | CHANGED | rc=0 >>
 22:36:51 up 22 min,  1 user,  load average: 0.32, 0.22, 0.26
localhost | CHANGED | rc=0 >>
 22:36:52 up 49 min,  1 user,  load average: 0.91, 0.35, 0.14


Q06:
web1 | CHANGED | rc=0 >>
Linux web1 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
web2 | CHANGED | rc=0 >>
Linux web2 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
web3 | CHANGED | rc=0 >>
Linux web3 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
balancer | CHANGED | rc=0 >>
Linux balancer 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
localhost | CHANGED | rc=0 >>
Linux mgmt 5.4.0-88-generic #99-Ubuntu SMP Thu Sep 23 17:29:00 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux


Q07: 
O bug acontece devido à variavel 'noc_ntpserver' estar definida como "server 0.europe.pool.ntp.org" e o ficheiro ntp.conf.j2 que usa variavel tinha escrito na linha 7 "server {{ noc_ntpserver }}", ficando assim na prática "server server 0.europe.pool.ntp.org". Por isso, apaguei a palavra "server", ficando apenas "{{ noc_ntpserver }}".


Q08: 
Não executou os RUNNING HANDLER [restart nginx] e [restart haproxy], que só se executam quando existem "changes". E neste caso as mudanças foram executadas na primeira vez que correu.


Q09:
O nome do servidor e o respetivo IP mudaram. Isto acontece devido a Load Balancer nos redirecionar para outro server com o objetivo de equilibrar e evitar sobrecarga no tráfego por server.


Q10:
Todos os requests retornaram com código HTTP 200 OK que é a resposta de status de sucesso.