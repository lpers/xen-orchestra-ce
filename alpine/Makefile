
.PHONY: all mrproper clean build push push_as_latest

# Get current xo component versions
XOSERVER := $(shell git ls-remote --tags --sort="v:refname" https://github.com/vatesfr/xen-orchestra.git | grep "xo-server-v" | tail -1 | cut -f2 | sed -rn "s/.*v//p")
XOWEB := $(shell git ls-remote --tags --sort="v:refname" https://github.com/vatesfr/xen-orchestra.git | grep "xo-web-v" | tail -1 | cut -f2 | sed -rn "s/.*v//p")
#
## If not defined used major xo-server version for global version
VERSION ?= $(shell echo $(XOSERVER) | sed -rn 's/([[:digit:]]+\.[[:digit:]]+)\..*/\1/p')
## Docker image name target
NAME ?= ezka77/xen-orchestra-ce

all: build

clean:
	docker container prune -f
	docker image prune -f

mrproper:
	docker image prune -af

build:
	docker build \
		-t $(NAME):$(VERSION) \
		--label "version=$(VERSION)" \
		--label "xo-server=$(XOSERVER)" \
		--label "xo-web=$(XOWEB)" .

push:
	docker push $(NAME):$(VERSION)

push_as_latest:
	docker tag $(NAME):$(VERSION) $(NAME):latest 
	docker push $(NAME):latest

