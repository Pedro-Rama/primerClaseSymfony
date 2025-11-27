# Makefile
#!/bin/bash

RABBIT = symfony-api-platform-rabbitmq
UID = $(shell id -u)

help: ## Show this help message
	@echo 'usage: make [target]'
	@echo
	@echo 'targets:'
	@egrep '^(.+)\:\ ##\ (.+)' ${MAKEFILE_LIST} | column -t -c 2 -s ':#'

run: ## Start the containers (rebuild with current UID before up)
	@echo "Building images with UID=$(UID)..."
	U_ID=$(UID) docker-compose build --no-cache --build-arg UID=$(UID)
	U_ID=$(UID) docker-compose up -d

stop: ## Stop the containers
	U_ID=$(UID) docker-compose stop

restart: ## Restart the containers
	$(MAKE) stop && $(MAKE) run

build: ## Rebuilds all the containers (passes UID build-arg)
	U_ID=$(UID) docker-compose build --no-cache --build-arg UID=$(UID)

ssh: ## ssh's into the container as the host UID
	U_ID=$(UID) docker exec -it --user $(UID) $(RABBIT) bash
