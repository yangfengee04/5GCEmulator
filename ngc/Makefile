################################################################################
# SPDX-License-Identifier: Apache-2.0
# Copyright (c) 2019 Intel Corporation
################################################################################


export GO111MODULE = on

.PHONY: build af nef test-unit-nef clean help
TMP_DIR:=$(shell mktemp -d)
BUILD_DIR ?=dist
CERTS_DIR ?=/etc/certs

VER:=1.0

build: af nef

af:
	mkdir -p "${BUILD_DIR}"
	GOOS=linux go build -o "${BUILD_DIR}/af" ./cmd/af
	cp ${BUILD_DIR}/af .

nef:
	mkdir -p "${BUILD_DIR}"
	GOOS=linux go build -o "${BUILD_DIR}/nef" ./cmd/nef
	cp ${BUILD_DIR}/nef .

test-unit-nef:
	rm -rf "${CERTS_DIR}"
	mkdir -p "${CERTS_DIR}"
	./scripts/genCerts.sh -t DNS -n localhost -m localhost
	mv root-ca-cert.pem root-ca-cert.srl root-ca-key.pem server-cert.pem server-key.pem server-request.csr "${CERTS_DIR}"
	ginkgo -v -cover ./pkg/nef

clean:
	rm -rf "${BUILD_DIR}"
	rm -rf "${CERTS_DIR}"
	rm -rf extfile.cnf

help:
	@echo "Please use \`make <target>\` where <target> is one of"
	@echo "Building Packages:"
	@echo "  build                  to build all"
	@echo "  af                     to build the AF"
	@echo "  nef                    to build the NEF"
	@echo "Executing Test:"
	@echo "  test-unit-nef          to run NEF unit tests with ginkgo"
