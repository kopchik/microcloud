FROM ubuntu:16.04

RUN apt update  \
 && apt -y upgrade  \
 && apt install -y --no-install-recommends python-pip git \
 && git clone --depth=1 /microcloud /home/microcloud  \
 && pip install --upgrade pip  \
 && pip install -r /home/microcloud/webserver/requirements.txt

CMD ["/home/microcloud/webserver/webserver.py"]
