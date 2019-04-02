all: 6 7

%:
	docker build -t centos-esgyn:$@ --build-arg osver=$@ .

.PHONY: all
