FROM ubuntu:16.04

RUN apt update  \
# TODO: uncomment && apt -y upgrade  \
 && apt install -y --no-install-recommends \
    git  \
    python-pip  \
    python-setuptools  \
 && git clone --depth=1 https://github.com/kopchik/microcloud.git /microcloud  \
 && pip install --upgrade pip  \
 && pip install -r /microcloud/webserver/requirements.txt

CMD ["/microcloud/webserver/webserver.py"]
