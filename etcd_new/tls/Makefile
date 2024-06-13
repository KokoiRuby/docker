.PHONY: cfssl ca req clean

# WSL need "" otherwise → /bin/sh: 1: Syntax error: "(" unexpected
CFSSL	= @env PATH="$(GOPATH)/bin:$(PATH)" cfssl
JSON	= env PATH="$(GOPATH)/bin:$(PATH)" cfssljson
# CFSSL	= @env PATH=$(GOPATH)/bin:$(PATH) cfssl
# JSON	= env PATH=$(GOPATH)/bin:$(PATH) cfssljson

all:  ca req

ca:
	mkdir -p certs
	$(CFSSL) gencert -initca config/ca-csr.json | $(JSON) -bare certs/ca

req:
	$(CFSSL) gencert \
	  -ca certs/ca.pem \
	  -ca-key certs/ca-key.pem \
	  -config config/ca-config.json \
	  config/server-csr.json | $(JSON) -bare certs/server
	$(CFSSL) gencert \
	  -ca certs/ca.pem \
	  -ca-key certs/ca-key.pem \
	  -config config/ca-config.json \
	  config/server-csr.json | $(JSON) -bare certs/peer
	$(CFSSL) gencert \
	  -ca certs/ca.pem \
	  -ca-key certs/ca-key.pem \
	  -config config/ca-config.json \
	  config/proxy-csr.json | $(JSON) -bare certs/proxy

clean:
	rm -rf certs

