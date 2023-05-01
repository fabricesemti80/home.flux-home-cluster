# notes

## connect to psql

- find the load balanced IP and connect to it (requieres <https://www.postgresql.org/docs/current/app-psql.html>)

```sh
❯ kubectl get service -n development postgres-lb
# NAME          TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
# postgres-lb   LoadBalancer   10.43.174.5   10.0.0.202    5432:30993/TCP   2d


❯ psql "sslmode=disable dbname=postgres user=postgres hostaddr=10.0.0.202"
# Password for user postgres:
# psql (14.7 (Ubuntu 14.7-0ubuntu0.22.04.1))
# Type "help" for help.

# postgres=#
```

Set password of the default user (password is in `cloudnative-pg` secret in 1Password)

```sh
# postgres@postgres-1:/$ psql
# psql (14.7 (Debian 14.7-1.pgdg110+1))
# Type "help" for help.

postgres=# sudo -u postgres psql postgres
postgres-# \password postgres
# Enter new password for user "postgres":
# Enter it again:
# postgres-#
```

### Plugin for kubectl


## useful things

- [init container](https://github.com/onedr0p/containers/tree/6c64f4a240468fee32f78bf171549058632ffbb6/apps/postgres-initdb)

- [cheat sheet](https://postgrescheatsheet.com/#/databases)

- [reset sa pw](https://serverfault.com/questions/110154/whats-the-default-superuser-username-password-for-postgres-after-a-new-install)