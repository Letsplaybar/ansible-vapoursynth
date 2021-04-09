# VapourSynth InstallScript
This script allows you to easily install vapoursynth on Linux.

____
## Configuration:
add the servers you want to install vapoursynth to your inventory/hosts

Example:
```
domain.tld ansible_host=127.0.0.1
```

create a directory with the hostname (domain.tld) and create a vars.yml in the directory

With the variable `user` you define the user via which ansible logs in. 

With the variable `ssh_port` you define the ssh port over which the server can be reached.

Example:
````yaml
#inventory/host_vars/domain.tld/vars.yml
user= ansible # default is root
ssh_port= 4489 # default is 22
````
___
## Initialise
In order for ansible to run error-free on the server, python must be installed on the server. to ensure this, 
execute the following command: `ansible-playbook -i inventory/hosts init.yml`

___
## Setup
Now all you have to do is run the playbook with the following command: `ansible-playbook -i inventory/hosts setup.yml`

___
## Additional
### You use a password to log in via ssh?
This is no problem just add the `--ask-pass` parameter to the command

### You see the following error?
`Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host.`<br>
You can easily solve this problem by manually establishing an ssh connection to the server.