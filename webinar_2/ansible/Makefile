#
# Tasks Makefile
# ==============
#
# Shortcuts for various tasks.
#

PRIVATE_KEY=~/.docker/machine/machines/demo-for-webinar.webitel.com/id_rsa

IP=`docker-machine ip demo-for-webinar.webitel.com`

dns-add:
	@(ansible-playbook -i hosts playbook.yml -u root -t add --extra-vars="IP=$(IP)" --private-key $(PRIVATE_KEY))

dns-del:
	@(ansible-playbook -i hosts playbook.yml -u root -t del --extra-vars="IP=$(IP)" --private-key $(PRIVATE_KEY))

copy-ssl:
	@(ansible-playbook -i hosts playbook.yml -u root -t ssl --private-key $(PRIVATE_KEY))

swap:
	@(ansible-playbook -i hosts playbook.yml -u root -t swap --private-key $(PRIVATE_KEY))

ping:
	@(ansible all -i hosts -m ping -u root --private-key $(PRIVATE_KEY))

setup:
	@(ansible all -i hosts -m setup -u root --private-key $(PRIVATE_KEY))

