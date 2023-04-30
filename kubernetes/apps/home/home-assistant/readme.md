# Homeassitant

Create db and user

```sh
postgres@postgres-1:/$ pgsql
bash: pgsql: command not found
postgres@postgres-1:/$ psql
psql (14.7 (Debian 14.7-1.pgdg110+1))
Type "help" for help.

postgres=# CREATE USER homeassistant WITH PASSWORD '<password from 1password>';
CREATE ROLE
postgres=# CREATE DATABASE homeassistant_db WITH OWNER homeassistant ENCODING 'utf8' TEMPLATE template0;
CREATE DATABASE
postgres=#
```

## References

<https://sigfried.be/blog/migrating-home-assistant-sqlite-to-postgresql/>