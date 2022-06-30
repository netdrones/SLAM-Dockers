ORG ?= netdrones
REPO ?= vins-mono

all: help

help:
	@echo ""
	@echo "------------------- Help Menu ------------------------"
	@echo ""
	@echo "     1. make build             - build all images"
	@echo "     2. make clean             - remove all images"
	@echo ""
	@echo "------------------------------------------------------"
	@echo ""

build:
	docker build --tag $(ORG)/$(REPO) -f ./Dockerfile ..

push: 
	docker push $(ORG)/$(REPO)

clean:
	docker rmi -f $(ORG)/$(REPO)
