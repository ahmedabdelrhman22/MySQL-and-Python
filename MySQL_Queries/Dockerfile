FROM mysql:latest
RUN chown -R mysql:mysql /docker-entrypoint-initdb.d/:ro
COPY ./MySQL_Queries /docker-entrypoint-initdb.d/:ro    

# ENV MYSQL_DATABASE crud_flask
# ENV MYSQL_USER dev
# ENV MYSQL_PASSWORD dev
# ENV MYSQL_RANDOM_ROOT_PASSWORD yes

