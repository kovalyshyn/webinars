FROM docker/whalesay:latest
RUN apt-get -y update && apt-get install -y fortunes fortunes-ru
RUN locale-gen ru_RU.UTF-8  
ENV LANG ru_RU.UTF-8  
CMD /usr/games/fortune | cowsay
