# Вебинар2: Первый запуск и настройка Webitel

- Общий обзор интерфейса администрирования Webitel
- Понятие домен и пользователь
- Подключение SIP телефона
- Подключение SIP шлюза
- Подключение пользователя
- Виды маршрутов, настройка простого входящего и исходящего маршрута

[Зарегистрироваться >](https://attendee.gotowebinar.com/register/3091924924554536449) 

### docker-machine

	docker-machine create -d digitalocean demo-for-webinar.webitel.com
	eval "$(docker-machine env demo-for-webinar.webitel.com)”
	docker-machine ip demo-for-webinar.webitel.com
	docker-machine ssh demo-for-webinar.webitel.com

### ansible

	ansible-playbook -i hosts playbook.ml -u root -t ssl

### docker-compose

	docker-compose up -d
	docker-compose ps
	docker-compose logs
