# Makefile

ASPEN_NAMESPACE=istio-system
ASPEN_VERSION=1.6.14-am2
# ASPEN_VERSION=1.6.12-am2
ASPEN_VALUES=./udf-values-${ASPEN_VERSION}.yaml
CHART_DIR=./aspenmesh-${ASPEN_VERSION}/manifests/charts
CERT_DIR=./aspenmesh-${ASPEN_VERSION}/samples/certs

ISTIO_VERSION=1.6.14

HELM_ARGS=
# HELM_ARGS=--debug

.PHONY: help install upgrade uninstall istioctl post-install post-uninstall

help: ## This help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z0-9_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

.DEFAULT_GOAL := help

install: istioctl  ## Install aspenmesh
	kubectl create ns ${ASPEN_NAMESPACE} || true
	kubectl create secret generic cacerts -n ${ASPEN_NAMESPACE} \
		--from-file=${CERT_DIR}/ca-cert.pem \
		--from-file=${CERT_DIR}/ca-key.pem \
		--from-file=${CERT_DIR}/root-cert.pem \
		--from-file=${CERT_DIR}/cert-chain.pem 
	helm install istio-base ${CHART_DIR}/base --namespace ${ASPEN_NAMESPACE} ${HELM_ARGS} || true
	sleep 10
	helm install istiod ${CHART_DIR}/istio-control/istio-discovery --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	sleep 120
	helm install istio-ingress ${CHART_DIR}/gateways/istio-ingress --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	helm install istio-egress ${CHART_DIR}/gateways/istio-egress --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	helm install istio-telemetry ${CHART_DIR}/istio-telemetry/grafana --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	kubectl apply -f ./post-install/

upgrade:  ## Upgrade aspenmesh
	helm upgrade istio-base ${CHART_DIR}/base --namespace ${ASPEN_NAMESPACE} ${HELM_ARGS} || true
	sleep 10
	helm upgrade istiod ${CHART_DIR}/istio-control/istio-discovery --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	sleep 20
	helm upgrade istio-ingress ${CHART_DIR}/gateways/istio-ingress --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	helm upgrade istio-egress ${CHART_DIR}/gateways/istio-egress --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	helm upgrade istio-telemetry ${CHART_DIR}/istio-telemetry/grafana --namespace ${ASPEN_NAMESPACE} --values ${ASPEN_VALUES} ${HELM_ARGS} || true
	kubectl apply -f ./post-install/

uninstall:  ## Uninstall aspenmesh
	kubectl delete -f ./post-install || true
	helm uninstall istio-telemetry --namespace ${ASPEN_NAMESPACE} || true
	helm uninstall istio-egress --namespace ${ASPEN_NAMESPACE} || true
	helm uninstall istio-ingress --namespace ${ASPEN_NAMESPACE} || true
	helm uninstall istiod --namespace ${ASPEN_NAMESPACE} || true
	helm uninstall istio-base --namespace ${ASPEN_NAMESPACE} || true
	kubectl delete ns ${ASPEN_NAMESPACE} || true

istioctl:  ## Install istioctl
	curl -sL https://istio.io/downloadIstioctl | ISTIO_VERSION=${ISTIO_VERSION} sh - && \
	sudo cp ~/.istioctl/bin/istioctl /usr/local/bin

post-install:  ## Post installation steps
	kubectl apply -f ./post-install/

post-uninstall:  ## Post uninstallation steps
	kubectl delete -f ./post-install/
