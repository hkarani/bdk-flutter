.DEFAULT_GOAL := help
PROJECTNAME=$(shell basename "$(PWD)")
# ##############################################################################
# # GENERAL
# ##############################################################################
VERSION="0.1.0"
.PHONY: help
help: makefile
	@echo
	@echo " Available actions in "$(PROJECTNAME)":"
	@echo
	@sed -n 's/^##//p' $< | column -t -s ':' |  sed -e 's/^/ /'
	@echo

## init: Install missing dependencies.
init:
	cargo install cargo-ndk
	@if [ $$(uname) == "Darwin" ] ; then cargo install cargo-lipo ; fi
## :

## all: Compile iOS, Android & Linux binaries
all: init jnilib unit-test zip
 
linux:
	@echo "[Creating linux binary ...] $@"
	docker stop bwcbuilder01 ; docker rm bwcbuilder01; \
	docker build --platform linux/x86_64 --build-arg USER_ID=$(shell id -u) --build-arg VERSION=$(VERSION) -t bwcbuilder . && \
	docker run --platform linux/x86_64 --name bwcbuilder01 -v $(shell pwd):/lwk-dart bwcbuilder ; docker stop bwcbuilder01 ; docker rm bwcbuilder01
	@echo "[Done ✅]"

jnilib:
	@echo "[Creating android jniLibs ...] $@"
	sh android.sh $(VERSION)
	@echo "[Done ✅]"

xcframework:
	@echo "[Creating bdk.xcframework ...] $@"
	sh darwin.sh $(VERSION)
	@echo "[Done ✅]"

unit-test:
	@echo "[Creating unit-test binaries ...] $@"
	sh unit-test.sh $(VERSION)
	@echo "[Done ✅]"

zip:
	@echo "[COMPRESSING BINARIES TO FLUTTER PACKAGE ...] $@"
	sh compress.sh $(VERSION)
	@echo "[Done ✅]"

# copy-build:
# 	@echo "[COPYING BINARIES TO FLUTTER PACKAGE ...] $@"
# 	sh copy.sh
# 	@echo "[Done ✅]"






