FROM mariadb:latest

COPY db.sql /docker-entrypoint-initdb.d/db.sql

ENV MYSQL_ROOT_PASSWORD test123 
ENV MYSQL_DATABASE testdb
ENV MYSQL_USER testdb
ENV MYSQL_PASSWORD lolmao12

EXPOSE 3306

CMD ["mysqld"]
