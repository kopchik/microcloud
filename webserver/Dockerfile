FROM ubuntu:16.04

ENV PREFIX /microcloud

RUN apt update  \
# TODO: uncomment && apt -y upgrade  \
 && apt install -y --no-install-recommends \
    git  \
    python-pip  \
    python-setuptools  \
 && rm /var/cache/apt/archives/*.deb  \
 && git clone --depth=1 https://github.com/kopchik/microcloud.git ${PREFIX}  \
 && pip install --upgrade pip  \
 && pip install -r ${PREFIX}/webserver/requirements.txt

CMD ${PREFIX}/webserver/webserver.py
