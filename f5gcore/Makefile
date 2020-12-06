# Makefile
F5GC_BASE_NAME          ?= f5gc-build-base
F5GC_GNBSIM_NAME        ?= f5gc-gnbsim
F5GC_AMF_NAME           ?= f5gc-amf
F5GC_SMF_NAME           ?= f5gc-smf
F5GC_UPF_NAME           ?= f5gc-upf
F5GC_NRF_NAME		 				?= f5gc-nrf
F5GC_AUSF_NAME          ?= f5gc-ausf
F5GC_NSSF_NAME          ?= f5gc-nssf
F5GC_PCF_NAME           ?= f5gc-pcf
F5GC_UDM_NAME           ?= f5gc-udm
F5GC_UDR_NAME           ?= f5gc-udr
F5GC_WEBUI_NAME		 			?= f5gc-webui

# With or without debug tools
ifeq ($(DEBUG), true)
DOCKER_ENV          ?= DOCKER_BUILDKIT=1
DOCKER_TAG          ?= v3.0.4-dbg
DOCKER_USER       	?= boeboe
DEBUG_TOOL_LIST     ?= vim strace net-tools curl netcat-openbsd bash tree tcpdump wget httpie bind-tools iproute2
DOCKER_BUILD_ARGS   ?= --rm --build-arg DEBUG_TOOLS=true --build-arg DEBUG_TOOL_LIST="${DEBUG_TOOL_LIST}"
else
DOCKER_ENV          ?= DOCKER_BUILDKIT=1
DOCKER_TAG          ?= v3.0.4
DOCKER_USER       	?= boeboe
DOCKER_BUILD_ARGS   ?= --rm --build-arg DEBUG_TOOLS=false
endif

BASE_IMAGE_NAME         ?= ${DOCKER_USER}/${F5GC_BASE_NAME}:${DOCKER_TAG}
GNBSIM_IMAGE_NAME       ?= ${DOCKER_USER}/${F5GC_GNBSIM_NAME}:${DOCKER_TAG}
AMF_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_AMF_NAME}:${DOCKER_TAG}
SMF_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_SMF_NAME}:${DOCKER_TAG}
UPF_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_UPF_NAME}:${DOCKER_TAG}
NRF_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_NRF_NAME}:${DOCKER_TAG}
AUSF_IMAGE_NAME         ?= ${DOCKER_USER}/${F5GC_AUSF_NAME}:${DOCKER_TAG}
NSSF_IMAGE_NAME         ?= ${DOCKER_USER}/${F5GC_NSSF_NAME}:${DOCKER_TAG}
PCF_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_PCF_NAME}:${DOCKER_TAG}
UDM_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_UDM_NAME}:${DOCKER_TAG}
UDR_IMAGE_NAME          ?= ${DOCKER_USER}/${F5GC_UDR_NAME}:${DOCKER_TAG}
WEBUI_IMAGE_NAME        ?= ${DOCKER_USER}/${F5GC_WEBUI_NAME}:${DOCKER_TAG}


.PHONY: build-base build-gnbsim build-amf build-smf build-upf build-nrf build-ausf build-nssf build-pcf build-udm build-udr build-webui
.PHONY: push-base push-gnbsim push-amf push-smf push-upf push-nrf push-ausf push-nssf push-pcf push-udm push-udr push-webui

.PHONY: help

help: ## This help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z0-9_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.DEFAULT_GOAL := help


build: build-base build-gnbsim build-amf build-smf build-upf build-nrf build-ausf build-nssf build-pcf build-udm build-udr build-webui ## Build all images locally
push: push-base push-gnbsim push-amf push-smf push-upf push-nrf push-ausf push-nssf push-pcf push-udm push-udr push-webui ## Push all images to dockerhub


### Base Image ###
build-base:
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${BASE_IMAGE_NAME} \
		--file ./images/${F5GC_BASE_NAME}/Dockerfile.alpine \
		./images/${F5GC_BASE_NAME}
push-base:
	${DOCKER_ENV} docker push ${BASE_IMAGE_NAME}


### GNBSIM - a 5G SA gNB/UE simulator for testing 5GC system ###
build-gnbsim:
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${GNBSIM_IMAGE_NAME} \
		--file ./images/${F5GC_GNBSIM_NAME}/Dockerfile.ubuntu18 \
		--build-arg TAG=${DOCKER_TAG} \
		--no-cache \
		./images/${F5GC_GNBSIM_NAME}
push-gnbsim:
	${DOCKER_ENV} docker push ${GNBSIM_IMAGE_NAME}


### AMF - Access and Mobility Management Function ###
build-amf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${AMF_IMAGE_NAME} \
		--file ./images/${F5GC_AMF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_AMF_NAME}
push-amf:
	${DOCKER_ENV} docker push ${AMF_IMAGE_NAME}


### SMF - Session Management Function ###
build-smf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${SMF_IMAGE_NAME} \
		--file ./images/${F5GC_SMF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_SMF_NAME}
push-smf:
	${DOCKER_ENV} docker push ${SMF_IMAGE_NAME}


### UPF - User Plane Function ###
build-upf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${UPF_IMAGE_NAME} \
		--file ./images/${F5GC_UPF_NAME}/Dockerfile.ubuntu18 \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_UPF_NAME}
push-upf:
	${DOCKER_ENV} docker push ${UPF_IMAGE_NAME}


### NRF - Network Repository Function ###
build-nrf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${NRF_IMAGE_NAME} \
		--file ./images/${F5GC_NRF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_NRF_NAME}
push-nrf:
	${DOCKER_ENV} docker push ${NRF_IMAGE_NAME}


### AUSF - Authentication Server Function ###
build-ausf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${AUSF_IMAGE_NAME} \
		--file ./images/${F5GC_AUSF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_AUSF_NAME}
push-ausf:
	${DOCKER_ENV} docker push ${AUSF_IMAGE_NAME}


### NSSF - Network slice selection function ###
build-nssf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${NSSF_IMAGE_NAME} \
		--file ./images/${F5GC_NSSF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_NSSF_NAME}
push-nssf:
	${DOCKER_ENV} docker push ${NSSF_IMAGE_NAME}


## PCF - Policy Control Function ###
build-pcf: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${PCF_IMAGE_NAME} \
		--file ./images/${F5GC_PCF_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_PCF_NAME}
push-pcf:
	${DOCKER_ENV} docker push ${PCF_IMAGE_NAME}


### UDM - Unified Data Manager Function ###
build-udm: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${UDM_IMAGE_NAME} \
		--file ./images/${F5GC_UDM_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_UDM_NAME}
push-udm:
	${DOCKER_ENV} docker push ${UDM_IMAGE_NAME}


### UDR - Unified Data Repository ###
build-udr: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${UDR_IMAGE_NAME} \
		--file ./images/${F5GC_UDR_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_UDR_NAME}
push-udr:
	${DOCKER_ENV} docker push ${UDR_IMAGE_NAME}


### WebUI ###
build-webui: build-base
	${DOCKER_ENV} docker build ${DOCKER_BUILD_ARGS} \
		--tag ${WEBUI_IMAGE_NAME} \
		--file ./images/${F5GC_WEBUI_NAME}/Dockerfile.alpine \
		--build-arg TAG=${DOCKER_TAG} \
		./images/${F5GC_WEBUI_NAME}
push-webui:
	${DOCKER_ENV} docker push ${WEBUI_IMAGE_NAME}


clean:
	docker rmi ${DOCKER_USER}/${F5GC_BASE_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_GNBSIM_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_AMF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_SMF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_UPF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_NRF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_AUSF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_NSSF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_PCF_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_UDM_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_UDR_NAME}:${DOCKER_TAG} || true
	docker rmi ${DOCKER_USER}/${F5GC_WEBUI_NAME}:${DOCKER_TAG} || true