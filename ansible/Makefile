VAULT_PASS ?= --ask-vault-pass

.PHONY: install deploy config firewall

install:
	pip install -r requirements.txt
	ansible-galaxy collection install -r requirements.yml

deploy:
	ansible-playbook playbook.yml $(VAULT_PASS)

config:
	ansible-playbook playbook.yml $(VAULT_PASS) --tags update_config

firewall:
	ansible-playbook playbook.yml $(VAULT_PASS) --tags iptables
