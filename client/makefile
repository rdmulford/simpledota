GOCMD=go
GOTEST=$(GOCMD) test
GORUN=$(GOCMD) run
BINARY_NAME=simpledota
VERSION	?= $(shell git describe --tags --always --dirty --match=v* 2> /dev/null || cat $(PWD)/.version 2> /dev/null || echo v0)
THIS_FILE := $(lastword $(MAKEFILE_LIST))

GREEN  := $(shell tput -Txterm setaf 2)
YELLOW := $(shell tput -Txterm setaf 3)
WHITE  := $(shell tput -Txterm setaf 7)
CYAN   := $(shell tput -Txterm setaf 6)
RESET  := $(shell tput -Txterm sgr0)

.PHONY: all test build fmt version

## all:
all: ## build test and run (make)
	@$(MAKE) -f $(THIS_FILE) clean fmt test coverage build run

## build:
build: ## build executable
	mkdir -p out/bin
	GO111MODULE=on $(GOCMD) build -mod vendor -o out/bin/$(BINARY_NAME) .

clean: ## remove build files
	rm -fr ./bin
	rm -fr ./out
	rm -f checkstyle-report.xml ./coverage.xml ./profile.cov

## test:
test: ## run all tests
	$(GOTEST) -v ./...

coverage: ## generate code coverage report
	$(GOTEST) -cover -covermode=count -coverprofile=profile.cov ./...
	$(GOCMD) tool cover -func profile.cov

## run:
run: ## start executable
	./out/bin/simpledota

## format:
fmt: ## format all go files
	gofmt -s -w .

## misc:
version: ## print the current working version
	@echo $(VERSION)

## help:
help: ## show this help
	@echo ''
	@echo 'Usage:'
	@echo '  ${YELLOW}make${RESET} ${GREEN}<target>${RESET}'
	@echo ''
	@echo 'Targets:'
	@awk 'BEGIN {FS = ":.*?## "} { \
		if (/^[a-zA-Z_-]+:.*?##.*$$/) {printf "    ${YELLOW}%-20s${GREEN}%s${RESET}\n", $$1, $$2} \
		else if (/^## .*$$/) {printf "  ${CYAN}%s${RESET}\n", substr($$1,4)} \
		}' $(MAKEFILE_LIST)
