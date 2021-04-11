# VapourSynth InstallScript
This script allows you to easily install vapoursynth on Linux.

___
# Requirements

The following requirements must be met for the system on which the script is to be executed afterwards:
- Python3
- ansible (It is best to install the latest version via `pip`)

---
# Preparing
drag the repo to a machine on which Ansible is installed ( doesn't have to be the same as the one on which you want to install VapourSynth)
`git clone https://github.com/Letsplaybar/ansible-vapoursynth.git`

___
## Configuration:
add the servers you want to install vapoursynth to your inventory/hosts

Example:
```
domain.tld ansible_host=127.0.0.1
```

Create a directory with the host name (domain.tld) in `inventory/host_vars/` and create a vars.yml in this directory

With the variable `user` you define the user via which ansible logs in. 

With the variable `ssh_port` you define the ssh port over which the server can be reached.

With the variable `python_version` you define the python version that is installed on the host.

Example:
````yaml
#inventory/host_vars/domain.tld/vars.yml

##System Vars
user: ansible # default is root (user require sudo)
ssh_port: 4489 # default is 22
python_version: python3.8 #default is python3.8

##Version Vars
##with these variables you determine which version is to be cloned for the install/compile
getnative_version: 2.2.1 #default is 2.2.1
zimg_version: v3.0 #default is v3.0
vapoursynth_version: R53-RC1 #default is R53-RC1
imagemagick_version: 7.0.11-6 #default is 7.0.11-6
ffms2_version: 2.40-RC1 #default is 2.40-RC1
descale_version: r6 #default is r6
````
___
## Initialise
In order for ansible to run error-free on the server, python must be installed on the server. to ensure this, 
execute the following command: `ansible-playbook -i inventory/hosts init.yml`
(This command does not need to be executed if the script is only to install Vapoursynth on the local machine.)

___
## Setup
Now all you have to do is run the playbook with the following command: `ansible-playbook -i inventory/hosts setup.yml --tags=vapoursynth`

___
## Install Plugins and Scripts
the script can also install the plugins `descale` and `ffms2` and the script `getnative` with the command: 
- `ansible-playbook -i inventory/hosts setup.yml --tags=getnative` (install descale, ffms2 and getnative)
- `ansible-playbook -i inventory/hosts setup.yml --tags=ffms2`
- `ansible-playbook -i inventory/hosts setup.yml --tags=descale`
- `ansible-playbook -i inventory/hosts setup.yml --tags=setup-all` (install the whole Script)

___
## Additional
### You use a password to log in via ssh?
This is no problem just add the `--ask-pass` parameter to the command

### You see the following error?
- Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host.

You can easily solve this problem by manually establishing an ssh connection to the server.

### VS errors
Depending on your operating system’s configuration, VapourSynth may not work out of the box with the default prefix of /usr/local. Two errors may pop up when running vspipe --version:
- “vspipe: error while loading shared libraries: libvapoursynth-script.so.0: cannot open shared object file: No such file or directory”

  This is caused by the non-standard location of libvapoursynth-script.so.0. Your dynamic loader is not configured to look in /usr/local/lib. One way to work around this error is to use the LD_LIBRARY_PATH environment variable:
  ```sh
  $ LD_LIBRARY_PATH=/usr/local/lib vspipe --version
  ```

-  “Failed to initialize VapourSynth environment”

  This is caused by the non-standard location of the Python module, vapoursynth.so. Your Python is not configured to look in /usr/local/lib/python3.x/site-packages. One way to work around this error is to use the PYTHONPATH environment variable:
  ```sh
  $ PYTHONPATH=/usr/local/lib/python3.x/site-packages vspipe --version
  ```
  Replace “x” with the correct number.