# Gitea

Create database user (inside postgres-s rw container)

```sh

# create user
CREATE ROLE gitea WITH LOGIN PASSWORD '<password from 1Pass>';

# create db
CREATE DATABASE gitea WITH OWNER gitea TEMPLATE template0 ENCODING UTF8 LC_COLLATE 'en_US.UTF-8' LC_CTYPE 'en_US.UTF-8';

```

Verify db/user present

```sh
postgres@postgres-1:/$ psql
psql (14.7 (Debian 14.7-1.pgdg110+1))
Type "help" for help.

postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 app       | app      | UTF8     | C           | C           |
 gitea     | gitea    | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 postgres  | postgres | UTF8     | C           | C           |
 template0 | postgres | UTF8     | C           | C           | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | C           | C           | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(5 rows)

postgres=# \du
                                       List of roles
     Role name     |                         Attributes                         | Member of
-------------------+------------------------------------------------------------+-----------
 app               |                                                            | {}
 gitea             |                                                            | {}
 postgres          | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 streaming_replica | Replication                                                | {}

postgres=#
```

## References

[Database preparation](https://docs.gitea.io/en-us/installation/database-prep/)