# Архитектура и установка Webitel

- Архитектура Webitel и взаимодействие компонент между собой
- Основные принципы работы Webitel
- Подготовка среды под Webitel
- Конфигурационные файлы и установка Webitel


### docker-machine

	docker-machine create -d virtual box demo
	eval "$(docker-machine env demo)"
	docker-machine ip demo
	docker-machine ssh demo

### docker

	docker run -t docker/whalesay cowboy Hello world!
	docker build -t demo .
	docker run -t demo

### docker-compose

	docker-compose up -d
	docker-compose ps
	docker-compose logs
