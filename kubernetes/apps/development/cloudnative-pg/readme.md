# notes

## connect to psql

- log in to the pod:

```sh
$ kubectl exec -n development --stdin --tty postgres-1 -- /bin/bash
# Defaulted container "postgres" out of: postgres, bootstrap-controller (init)

# postgres@postgres-1:/$ psql
# psql (14.7 (Debian 14.7-1.pgdg110+1))
# Type "help" for help.

$ postgres=#
```
<!-- ***or**

Find your cluster

```sh
kubectl get Cluster
NAME       AGE   INSTANCES   READY   STATUS                     PRIMARY
postgres   11h   3           3       Cluster in healthy state   postgres-1
```

Install the plugin (see next section)

Connect to the cluster

```sh
``` -->

### Plugin for kubectl

[cnpg](https://github.com/EnterpriseDB/kubectl-cnp)

## useful things

- [init container](https://github.com/onedr0p/containers/tree/6c64f4a240468fee32f78bf171549058632ffbb6/apps/postgres-initdb)

- [cheat sheet](https://postgrescheatsheet.com/#/databases)