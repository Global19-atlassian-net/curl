TAG=curl

all: build run

build:
	docker build -t $(TAG) .

run:
	docker run -it $(TAG) -v https://www.zmartzone.eu:4433
