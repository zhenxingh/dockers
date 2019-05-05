all: 6 7

%:
	docker build -t centos-esgyn:$@ --build-arg osver=$@  --build-arg user=${USER} .

.PHONY: all
