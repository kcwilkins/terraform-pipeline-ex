.SILENT:
VERSION = v1
USERNAME = kcwilkins
IMAGE_NAME ?= ghcr.io/kcwilkins/terraform-pipeline-ex/cicd:$(VERSION)
TAG = $(VERSION)

.PHONY: help
help:
	echo "Add help and command list here"

.PHONY: build
build:
	docker build -t $(IMAGE_NAME) .

.PHONY: test
test:
	docker run --rm -it --entrypoint="terraform" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="ansible" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="aws" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="ssh" $(IMAGE_NAME) -V
	docker run --rm -it --entrypoint="git" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="python" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="python3" $(IMAGE_NAME) --version
	docker run --rm -it --entrypoint="docker" $(IMAGE_NAME) --version

.PHONY: shell
shell:
	docker run --rm -it --entrypoint="bash" -v ~/.aws:/root/.aws -v $(shell pwd):/opt/app $(IMAGE_NAME)

.PHONY: push
push:
	# https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry

	echo "To create a PAT - https://github.com/settings/tokens"
	echo "Provide the login into GitHub Docker Repository using the following syntax:"
	echo "export CR_PAT=\"<your personal access token>\""
	echo "export USERNAME=\"<your github username>\""
	echo ""
	echo "Calling: echo ${CR_PAT} | docker login ghcr.io -u ${USERNAME} --password-stdin"
	echo ${CR_PAT} | docker login ghcr.io -u ${USERNAME} --password-stdin
	echo "Pushing ${IMAGE_NAME}"
	docker push ${IMAGE_NAME}

