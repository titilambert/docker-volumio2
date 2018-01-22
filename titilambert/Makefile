build:
	docker build -t titilambert/volumio2:latest -f Dockerfile .

run:
	docker run -it --rm --name volumio -e HOST=http://127.0.0.1:3000  -v `pwd`/volumio:/home/volumio/ -v `pwd`/data:/data --net=host titilambert/volumio2:latest

push:
	docker push titilambert/volumio2:latest
