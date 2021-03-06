.PHONY: build

HOSTIP=$(shell ip route get 1 | awk '{print $$NF;exit}')

default:
	@echo ""
	@echo "Targets:"
	@echo ""
	@echo "  build                  Build plus + community images"
	@echo "  build_plus             Build plus image"
	@echo "  build_community        Build community image"
	@echo "  test                   Test plus + community images"
	@echo "  test_plus              Test plus image"
	@echo "  test_community         Test community image"
	@echo "  version                Get CB version in plus + community images"
	@echo "  version_plus           Get CB version in plus image"
	@echo "  version_community      Get CB version in community image"
	@echo "  publish                Publish plus + community images"
	@echo "  publish_plus           Publish plus image"
	@echo "  publish_community      Publish community image"
	@echo ""

build: build_plus build_community

build_plus:
	# enforce auto-generating a new key when using the images
	sudo rm -f node/.crossbar/node.key
	time sudo docker build -t crossbario/crossbar:plus -f Dockerfile.plus .

build_community:
	# enforce auto-generating a new key when using the images
	sudo rm -f node/.crossbar/node.key
	time sudo docker build -t crossbario/crossbar:community -t crossbario/crossbar:latest -f Dockerfile.community .


test: test_plus test_community

test_plus:
	sudo docker run --rm -it -p 8080:8080 --name crossbar crossbario/crossbar:plus

test_community:
	sudo docker run --rm -it -p 8080:8080 --name crossbar crossbario/crossbar:community


version: version_plus version_community

version_plus:
	sudo docker run --rm --entrypoint=/opt/crossbar/bin/crossbar -it crossbario/crossbar:plus version

version_community:
	sudo docker run --rm --entrypoint=/usr/local/bin/crossbar -it crossbario/crossbar:community version


publish: publish_plus publish_community

publish_plus: build_plus
	sudo docker push crossbario/crossbar:plus

publish_community: build_community
	sudo docker push crossbario/crossbar:latest
	sudo docker push crossbario/crossbar:community


list:
	sudo docker images
