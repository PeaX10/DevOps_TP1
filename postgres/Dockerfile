FROM postgres:11.6-alpine

ENV POSTGRES_DB=db_tp1 \
POSTGRES_USER=guest \
POSTGRES_PASSWORD=guest_pwd

ADD 1_create_table.sql /docker-entrypoint-initdb.d
ADD 2_insert_data.sql /docker-entrypoint-initdb.d
