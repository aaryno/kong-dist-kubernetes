Kong can easily be provisioned to Minikube cluster using the following steps:

1.  **Deploy Kubernetes via Minikube**
    
    You need [minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) and
    [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
    command-line tools installed and set up to run deployment commands.

    Using the `minikube` command, deploy a Kubernetes cluster.

    ```bash
    $ minikube start
    ```

    By now, you have provisioned a Kubernetes managed cluster locally.

2. **Deploy a Kong supported database**
  
    Before deploying Kong, you need to provision a Cassandra or PostgreSQL pod.

    For Cassandra, use the `cassandra.yaml` file from this repo to deploy a
    Cassandra `Service` and a `ReplicationController` in the cluster:  

    ```bash
    $ kubectl create -f cassandra.yaml
    ```
    
    For PostgreSQL, use the `postgres.yaml` file from the kong-dist-kubernetes 
    repo to deploy a PostgreSQL `Service` and a `ReplicationController` in the
    cluster:

    ```bash
    $ kubectl create -f postgres.yaml
    ```

3. **Deploy Kong**

    Using the `kong_<postgres|cassandra>.yaml` file from this repo, deploy
    a Kong `Service` and a `Deployment` to the cluster created in the last step:
    
    ```bash
    $ kubectl create -f kong_<postgres|cassandra>.yaml
    ```

4. **Verify your deployments**

    You can now see the resources that have been deployed using `kubectl`:

    ```bash
    $ kubectl get rc
    $ kubectl get deployment
    $ kubectl get pods
    $ kubectl get services
    $ kubectl get logs <pod-name>
    ```

    Once the kong-admin and kong-proxy pods are started, you
    can test Kong by making the following requests:

    ```bash
    $ curl $(minikube service --url kong-admin)
    $ curl $(minikube service --url kong-proxy|head -n1)
    ```

    It may take up to 3 minutes for all services to come up.

5. **Using Kong**

    Quickly learn how to use Kong with the 
    [5-minute Quickstart](https://getkong.org/docs/latest/getting-started/quickstart/).

## Important Note

When deploying into a Kubernetes cluster with Deployment Manager, it is
important to be aware that deleting `ReplicationController` Kubernetes objects
**does not delete its underlying pods**, and it is your responisibility to
manage the destruction of these resources when deleting or updating a
`ReplicationController` in your configuration.


## Enterprise Support

Support, Demo, Training, API Certifications and Consulting available at http://getkong.org/enterprise.

[kong-logo]: http://i.imgur.com/4jyQQAZ.png
[website-url]: https://getkong.org/
[website-badge]: https://img.shields.io/badge/GETKong.org-Learn%20More-43bf58.svg
[documentation-url]: https://getkong.org/docs/
[documentation-badge]: https://img.shields.io/badge/Documentation-Read%20Online-green.svg
[gitter-url]: https://gitter.im/Mashape/kong
[gitter-badge]: https://img.shields.io/badge/Gitter-Join%20Chat-blue.svg
[mailing-list-badge]: https://img.shields.io/badge/Email-Join%20Mailing%20List-blue.svg
[mailing-list-url]: https://groups.google.com/forum/#!forum/konglayer

