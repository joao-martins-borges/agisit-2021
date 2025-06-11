Q01: Interpret the Vagranfilethat will be used, explaining, in your own words, what you think the “instructions” in it are supposed to do.

(R1):Vagrant is a tool designed to automate the building of virtual machines. The Vagrantfile is the file inside the Webfront directory that specifies the configuration of the virtual machines we want to build. In this file, we have 3 major sections: the configuration of the Management Node virtual machine (mgmt), the configuration of the Load Balancer virtual machine (balancer) and the specification of the web servers’ virtual machines. In each section, we configure the operating system, hostname, network IP, hardware specifications, etc…

Q02: Analyze briefly the bootstrap.sh the host_ip.sh and the host_ssh.sh to interpret their purpose.

(R2):The bootstrap.sh script oversees the installation of the dependencies needed inside each virtual machine, the host_ip.sh oversees the attribution of network address to hostname for each virtual machine and the host_ssh.sh script oversees the allowance of key based authentication.

Q03: In what differs calling ansible --version from any directory, when compared by calling it from the Project directory you will launch?

(R3):The only difference is that the path regarding the config file changes from /etc/ansible.cfg to /home/vagrant/tools/ansible.cfg. This happens because the tools directory already has a ansible configuration file in it, and so when we run the ansible --version command on this directory, we will get the confing file inside tools, while in any other directory, the config file for ansible is located in /etc/ansible.cfg.

Q04: After changing the Vagrantfile to accommodate it for launching more web servers, which were the modifications in the files of the Project that you have done? (just tell the name of the file and the lines that were changed)

(R4):
File: webfront/Vagrantfile		Line(s): 82	
File: webfront/tools/inventory.ini	Line(s): 9, 20, 28


Q05: Write the result of the command ansible all -m shell -a "uptime" for your modified infrastructure.

(R5):
vagrant@mgmt:~/tools$ ansible all -m shell -a "uptime"
localhost | CHANGED | rc=0 >>
 15:26:40 up  2:20,  1 user,  load average: 0.00, 0.00, 0.00
balancer | CHANGED | rc=0 >>
 15:26:43 up  2:19,  1 user,  load average: 0.64, 0.15, 0.04
web1 | CHANGED | rc=0 >>
 15:26:43 up  2:18,  1 user,  load average: 0.00, 0.00, 0.00
web2 | CHANGED | rc=0 >>
 15:26:43 up  2:17,  1 user,  load average: 0.16, 0.04, 0.01
web3 | CHANGED | rc=0 >>
15:26:43 up  2:11,  1 user,  load average: 0.08, 0.02, 0.01


Q06: Write the result of the command ansible all -m shell -a "uname -a" for your modified infrastructure.

(R6):
vagrant@mgmt:~/tools$ ansible all -m shell -a "uname -a"
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


Q07: When deploying the Network Time Protocol (NTP) you have changed the Reference Time Servers, by modifying the Playbook in order to include a "variable" to be replaced in the NTP configuration file when Ansible runs that Playbook. However, there was a "bug" inadvertently written for this new procedure. Did you find the Bug? What corrections were made, for the NTP service to run?

(R7):
When deploying the NTP, when we made Ansible run the ntp-template Playbook, I was expecting the procedure to run smoothly, however, the npt service was not installed correctly.Therefore, something had to be wrong in the configuration of this service. I noticed that the ntp-install.yml and the ntp-template.yml differ only in the “-hosts” section, where the ntp-template.yml specifies a server variable while the ntp-install.yml doesn’t. That means that the main issue has something to do with this hard coding of the server in the template Playbook. Going further, I recognized that in the ntp.conf.j2 file, in line number 7 was specified: “server {{ noc_ntpserver }}”. This in conjunction with line 7 of the template Playbook file meant that the server variable in the configuration would result in something of the like: “server server 0.europe.pool.ntp.org”.
Finally, in order to correct this issue, I just had to remove in the template Playbook variable or the ntp.conf.j2 file the word “server” that I specified earlier so as not to repeat itself when configuring the NTP installation.


Q08: You ran the site_(docker/vbox).yml Playbook. After running it for the second time, in case there were nor errors the would prevent all tasks to complete, can you describe: What happened? What did you find different (or not)?

(R8):
When running the ansible-playbook site_vbox.yml for the second time, the main difference at hand is that the TASKS that occured in the first command, also occured in the second command. However, the Running Handlers did not. That is because Running handlers are tasks that only run when something changes in the machine. In the first command, 2 running handlers happended since it was the first time running the command. The second time, done right after, nothing changed so there was no need to run the running handler tasks.

Q09: When the system was deployed, when hitting the refresh button on the web browser (forcing with the Shift key): Did something change?

(R9):
When the system was deployed, when hitting the refresh button (forcing with the Shift key), we would cycle through the connection of the Load Balancer with each of the running web server virtual machines, changing the IP address that was displaying to the corresponding IP of the web server, and the hostname of the corresponding machine.   

Q10: When using the Benchmarking tool, when using the concurrency parameter (-c) to a value still sustainable, What did you observe in the results of the Benchmark, were tehre errors, or failed request? (just a brief interpretation).

(R10):
For each time I ran the command and changed the concurrency value from 2 to 5 and then 10. Each time I ran it, the 90th, 95th and 99th percentile would rise considerably. The average also went up considerably, from 6,2ms with concurrency value of 2 and going to 40,4ms with the concurrency value of 40ms. The min and max values would also increase in each instance, giving that the average also increased. No requests seemed to fail, and no error were found.
