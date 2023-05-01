# Homeassitant

Create db user

```sh

postgres@postgres-1:/$ psql
# psql (14.7 (Debian 14.7-1.pgdg110+1))
# Type "help" for help.

postgres=# CREATE USER homeassistant WITH PASSWORD '<password from 1password>';
CREATE ROLE

postgres=#
```

- Ensure the `super user password` == `POSTGRES_SUPER_PASS`
- Ensure the created user name == `POSTGRES_USER`
- Ensure the created user password == `POSTGRES_PASS`
(these values should come from the `hass` secret in 1Password!)

With above, the db should be created by the deployment's `init-db` container.

## Configuration


## References

<https://sigfried.be/blog/migrating-home-assistant-sqlite-to-postgresql/>

<https://www.home-assistant.io/examples/>

<https://www.home-assistant.io/docs/configuration/>