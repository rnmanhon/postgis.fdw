FROM mdillon/postgis

RUN apt-get update && apt-get install -y curl  pkg-config libossp-uuid-dev wget ca-certificates git make libpq-dev python-psycopg2  postgresql-server-dev-9.5  python-dev python-pip && rm -rf /var/lib/apt/lists/* &&  pip install pgxnclient

RUN sed -i 's/ -shared / -shared -fPIC /g' /usr/lib/postgresql/9.5/lib/pgxs/src/Makefile.shlib

#Compiling & Installing MongoDB Forwarder module
# RUN mkdir -p /home/root && cd /home/root && git clone https://github.com/EnterpriseDB/mongo_fdw  --recursive && cd /home/root/mongo_fdw/ && ./autogen.sh --with-master && make install && ldconfig

RUN mkdir -p /home/root && cd /home/root && git clone https://github.com/EnterpriseDB/mongo_fdw  --recursive && cd /home/root/mongo_fdw/ && ./autogen.sh --with-master && make clean && make install && ldconfig