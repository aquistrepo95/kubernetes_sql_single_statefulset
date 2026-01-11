# kubernetes_sql_single_statefulset

## Functional Kubernetes infrastructure project running a single Statefulset replica for mysql:8.0

## This project showcases the following concepts in kubernetes:
* Deploying a stateful application using the Statefulset controller.
* Static storage creation using hostpath volumes NB: It is recommended to use Local volumes for more durability OR dynamic storage through storage classes for better provisioning.
* Persistent volumes and Persistent Volume claims for replicas (in this project for the mysql:8.0 replica)
* Config-maps to provide configuration information to both replicas i.e Deployment and Statefulset
* Secret to provide sensitive information to Statefulset and Deployment replicas.

## Built with:
### Stateful Application:
 * mysql:8.0
### Kubernetes
 * minikube
 * kubectl
### Docker

## This section will describe how to get the project running on your local machine.
* Set up Volume on the Node:
  ```
   % minikube ssh
   % sudo cd ../../mnt/data
   % sudo mkdir -p sql-data
  ```
* Apply Persistent Volume - open another terminal, navigate to the directory you cloned this repo into and type the following commands:
  ```
   % kubectl apply -f persistent_vol.yaml
     NAME                CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                     STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
     gettingstarted-pv   5Gi        RWO            Delete           Available                             <unset>                                         10d
  ```
* Apply Config-maps and secrets:
  ```
   % kubectl apply -f gettingstarted_config_sql.yaml
   % kubectl get cm
     NAME                            DATA   AGE
     gettingstarted-configvals-sql   1      9d
   % kubectl apply -f gettingstarted_secret-sql.yaml
   % kubectl get secret
     NAME                         TYPE     DATA   AGE
     gettingstarted-secrets-sql   Opaque   1      9d
  ```
* Apply Statefulset:
  ```
  % kubectl apply -f getting_started_sql.yaml
  % kubectl get sts,po
    NAME                                  READY   AGE
    statefulset.apps/gettingstarted-sql   1/1     8d

    NAME                                       READY   STATUS    RESTARTS       AGE
    pod/gettingstarted-sql-0                   1/1     Running   3 (6h9m ago)   8d
  ```
  
