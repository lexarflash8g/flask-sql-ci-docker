#Import and expose environment variables
cnf ?= .env_vars
include $(cnf)
export $(shell sed 's/=.*//' $(cnf))

.PHONY: init

help:
	@echo
	@echo "Usage: make TARGET"
	@echo
	@echo "Dockerization of the sloria/cookiecutter-flask template repo"
	@echo
	@echo "Targets:"
	@echo "	init 		initialize your app code base from the sloria/cookiecutter-flask repo"
	@echo "	init-purge 	clean up generated code"

#Generate project codebase form GitHub using cookiecutter
init:
	envsubst <docker/init/cookiecutter.template.yml >docker/init/cookiecutter.yml
	docker-compose -f docker/init/docker-compose.yml up -d --build
	docker cp flask-sql-ci-initiator:/root/$(APP_NAME) ./$(APP_NAME)
	docker-compose -f docker/init/docker-compose.yml down
	rm docker/init/cookiecutter.yml

#Remove the generated code, use this before re-running the `init` target 
init-purge: 
	sudo rm -rf ./$(APP_NAME)