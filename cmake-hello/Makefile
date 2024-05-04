CMAKE ?= cmake

.PHONY: all
all: install

.PHONY: cleaninstall
cleaninstall: | clean install

.PHONY: configure
configure:
	@${CMAKE} -S . -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install -DINSTALL_GTEST=OFF

.PHONY: build
build: | configure
	@${CMAKE} --build build --config Release -j -v

.PHONY: test
test: | build
ifneq ($(OS),Windows_NT)
	@${CMAKE} --build build --config Release --target test -j -v
else
	@${CMAKE} --build build --config Release --target RUN_TESTS -j -v
endif

.PHONY: install
install: | test
	@${CMAKE} --build build --config Release --target install -v

.PHONY: clean
clean:
	@rm -rf build install