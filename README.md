# Deployment PostgreSQL to Kubernetes

🚢[Docker Image](https://hub.docker.com/_/postgres)

### Running On Premise 
- Ubuntu 20.04 LTS
- Storage Class Rook Ceph Block

### Specification My Cluster
- Master node vCPU 2 Memory 8Gb
- Worker node1 vCPU 2 Memory 4Gb
- Worker node2 vCPU 2 Memory 4Gb
- Worker node3 vCPU 2 Memory 4Gb

### Installation
1. Clone this project.
    ```sh
    $ git clone https://github.com/skyapps-id/K8s-PostgreSQL-Standalone.git 
    ```

2. Create the Secret resource in your Kubernetes cluster by using the kubectl apply command.
    ```sh
    $ cd  K8s-PostgreSQL-Standalone/
    $ kubectl apply -f configmap-and-secret-postgresql.yml
    ```
     - NOTE Secrets data are stored as Base64 encoded strings. command : ```$ echo -n 'example' | base64 ``` example database ``postgresdb`` user ``admindb`` password is ``adminpassword``

4. Creating a Presistance Volume Claim resource in your Kubernetes cluster by using the kubectl apply command.
    ```sh
    $ kubectl apply -f persistent-volume-claim-postgresql.yaml
    ```

5. Deployment PostgreSQL resource in your Kubernetes cluster by using the kubectl apply command.
    ```sh
    $ kubectl apply -f deployment-postgresql.yaml
    ```

6. Creating a Service resource in your Kubernetes cluster by using the kubectl apply command.
    ```sh
    $ kubectl apply -f service-postgresql.yaml
    ```

7. Testing PostgreSQL Service.
    ```sh
    $ kubectl get pods
    NAME                          READY   STATUS    RESTARTS   AGE
    postgresql-54fb94c994-ck8tc   1/1     Running   0          22m

    $ kubectl exec -it postgresql-54fb94c994-ck8tc -- psql -h postgresql -U admindb --password -p 5432 postgresdb
    Password:
    psql (13.2 (Debian 13.2-1.pgdg100+1))
    Type "help" for help.

    postgresdb=# \l
                                    List of databases
        Name    |  Owner  | Encoding |  Collate   |   Ctype    |  Access privileges
    ------------+---------+----------+------------+------------+---------------------
    postgres   | admindb | UTF8     | en_US.utf8 | en_US.utf8 |
    postgresdb | admindb | UTF8     | en_US.utf8 | en_US.utf8 |
    template0  | admindb | UTF8     | en_US.utf8 | en_US.utf8 | =c/admindb         +
                |         |          |            |            | admindb=CTc/admindb
    template1  | admindb | UTF8     | en_US.utf8 | en_US.utf8 | =c/admindb         +
                |         |          |            |            | admindb=CTc/admindb
    (4 rows)

    postgresdb=#
    ```

### Licence

This work is under [MIT](LICENCE) licence.