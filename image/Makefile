.SILENT:
VERSION = v1
IMAGE_NAME ?= registry.gitlab.com/kcwest/octain/infra/cicd:$(VERSION)
TAG = $(VERSION)
OCTAIN_PROD_PROFILE = aws_octain_prod
OCTAIN_PROD_ECR = 867364193007.dkr.ecr.us-west-1.amazonaws.com
OCTAIN_PROD_IMAGE ?= 867364193007.dkr.ecr.us-west-1.amazonaws.com/octain-server:latest
OCTAIN_ECR_IMAGE ?= registry.gitlab.com/kcwest/octain/infra/octain-ecs:latest

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
	docker login registry.gitlab.com
	echo "Pushing ${IMAGE_NAME}"
	docker push ${IMAGE_NAME}

.PHONY: pull_octain_prod
pull_octain_prod:
	aws ecr get-login-password --profile ${OCTAIN_PROD_PROFILE} --region us-west-1 | docker login --username AWS --password-stdin ${OCTAIN_PROD_ECR}
	echo "Pulling ${OCTAIN_PROD_IMAGE}"
	docker pull ${OCTAIN_PROD_IMAGE}

.PHONY: push_ecs
push_ecs:
	echo "Retagging ${OCTAIN_PROD_IMAGE} to ${OCTAIN_ECR_IMAGE}"
	docker tag ${OCTAIN_PROD_IMAGE} ${OCTAIN_ECR_IMAGE}
	docker login registry.gitlab.com
	echo "Pushing ${OCTAIN_ECR_IMAGE}"
	docker push ${OCTAIN_ECR_IMAGE}
