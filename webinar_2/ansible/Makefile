#
# Tasks Makefile
# ==============
#
# Shortcuts for various tasks.
#

PRIVATE_KEY=~/.docker/machine/machines/demo-for-webinar.webitel.com/id_rsa

dns_add:
	@(ansible-playbook -i hosts playbook.yml -u root -t add --private-key $(PRIVATE_KEY))

dns_del:
	@(ansible-playbook -i hosts playbook.yml -u root -t del --private-key $(PRIVATE_KEY))

ssl:
	@(ansible-playbook -i hosts playbook.yml -u root -t ssl --private-key $(PRIVATE_KEY))

ping:
	@(ansible all -i hosts -m ping -u root --private-key $(PRIVATE_KEY))

setup:
	@(ansible all -i hosts -m setup -u root --private-key $(PRIVATE_KEY))

