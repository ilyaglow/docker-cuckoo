FROM blacktop/cuckoo:latest
LABEL repository "https://github.com/blacktop/docker-cuckoo"

ENV CUCKOO_PACKAGE_PATH=/usr/lib/python2.7/site-packages/cuckoo
ENV CUCKOO_FORK_REPO=ilyaglow/cuckoo/remotevbox-machinery

# Raw ugly patching cuckoo package
RUN wget -O $CUCKOO_PACKAGE_PATH/machinery/virtualbox_websrv.py https://raw.githubusercontent.com/$CUCKOO_FORK_REPO/cuckoo/machinery/virtualbox_websrv.py \
  && wget -O $CUCKOO_PACKAGE_PATH/common/config.py https://raw.githubusercontent.com/$CUCKOO_FORK_REPO/cuckoo/common/config.py \
  && pip install remotevbox
