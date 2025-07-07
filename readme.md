## Directory Structure
->Ansible 
	ansible.cfg {this holds a basic minimal config}
	inventory.yml {the standard inventory path for all the playbooks}
	secrets.yml {holds the admin password, root password, and service passwords}
	->handlers
		handlers.yml {this just has all the handlers that get called}
	->keys
		id_rsa4096 {the key which is used for auth after first connection}
		id_rsa4096.pub {the pub key placed in authorized_hosts on first connection}
	->playbooks
		auth_setup.yml {setup ansible service account, place ssh keys}
		install_packages.yml {install necessary packages (like fail2ban & libpam & ufw)}
		user_hardening.yml {restrict sudo, set root password}
		firewall_config.yml {config ufw}
		metasploitable2.yml {targeted vulnerability patching}

---------------------------------------

## Examples
Example workflow: 
1. Setup Keys on host:
	* if given a private key + user for first login "sudo ansible-playbook playbooks/auth_setup.yml --ask-vault-pass --private-key ~/.ssh/id_ed25519 -u rato1426 -K"
	* if given user password uncomment the vars_prompt in auth_setup and run sudo "ansible-playbook playbooks/auth_setup.yml --ask-vault-pass -K "
2. Install Packages: 
	* ansible-playbook playbooks/install_packages.yml --ask-vault-pass 
3. User Hardening:
	* ansible-playbook playbooks/user_hardening.yml --ask-vault-pass 
4. Firewall Config
	* ansible-playbook playbooks/firewall_config.yml --ask-vault-pass 
5. fail2ban:
    *   ansible-playbook playbooks/fail2ban.yml --ask-vault-pass 
6. Metasploitable2:
	* ansible-playbook playbooks/metasploitable2.yml --ask-vault-pass 

## Cheeky one liner (almost)
1. sudo ansible-playbook playbooks/auth_setup.yml --ask-vault-pass --private-key ~/.ssh/id_ed25519 -u rato1426 -K
2. uncomment necessary / update inventory ;^) 
3. ansible-playbook playbooks/install_packages.yml --ask-vault-pass && ansible-playbook playbooks/install_packages.yml --ask-vault-pass && ansible-playbook playbooks/user_hardening.yml --ask-vault-pass && ansible-playbook playbooks/firewall_config.yml --ask-vault-pass && ansible-playbook playbooks/fail2ban.yml --ask-vault-pass && ansible-playbook playbooks/metasploitable2.yml --ask-vault-pass
----

## Ansible Tips
-K prompt for sudo PW
-k prompt for normal pw
-u specify ssh user
--ask-vault-pass specify the vault password, some documentation includes pw.txt instead but i prefer doing it in the cli 

## Bash Tips
This is important if you're putting passwords in the cli or echoing passwords in the terminal
set +o history # temporarily turn off history
set -o history # turn it back on
we can also do this in the ansible tasks too

STILL TODO:
[x] auth setup 
[x] fail2ban setup
[x] install useful packages
[x] metasploitable2 specific vulns
[x] schedulers
[x] user hardening
[]Update all playbook tasks with the 'register: var' and print output at the end same as im doing in install packages
[]look into aide for HIDS
[]look into auditd for enhanced auditing
[]look into configuring smtp, maild & fail2ban to notify me of attempted SSH via email
[] aide config
[]