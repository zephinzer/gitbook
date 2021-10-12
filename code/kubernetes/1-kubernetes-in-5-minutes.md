# 1/ Kubernetes in 5 Minutes

> As published on [Medium](https://joeir.medium.com/kubernetes-in-five-minutes-18aa4101544f) and [Dev.to](https://dev.to/zephinzer/kubernetes-in-five-minutes-31m6)



To me, the areas that Kubernetes natively accounts for in logical — _imo_ — order are:

1. **Hardware** – Node
2. **Orchestration** – Deployment, Job, CronJob, StatefulSet, DaemonSet
3. **Configuration** – ConfigMap, Secret
4. **Persistence** — PersistentVolumeClaim, PersistentVolume
5. **Execution** – Pod, ReplicaSet
6. **Access Control** – Namespace, ServiceAccount, Role, ClusterRole
7. **Exposure** – Service, Ingress

## Hardware <a href="0f59" id="0f59"></a>

All software has one fundamental dependency: a machine to run on. We need **hardware** whether it’s our own computers or someone else’s and this abstraction comes in the form of a Node resource type.

* [**Node**](https://kubernetes.io/docs/concepts/architecture/nodes/): Represents a logical machine/VM (eg. your computer, ec2 instance, a droplet).

## Orchestration <a href="761f" id="761f"></a>

After we’ve defined our hardware, we need a plan on how to deploy our application. Will it be a single instance? Do we need maybe two? When we need to update our application, should it be done one by one or all at once? Do we want it to be deployed across all our Nodes? Should it be run once every day at 3 AM? We need to **orchestrate** the deployment.

* [**Deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): Long-standing workloads (eg. a server)
* [**Job**](https://kubernetes.io/docs/concepts/workloads/controllers/job/): One-off workload (eg. a shell script/database migration task)
* [**CronJob**](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/): Periodically-run one-off workload (eg. data sync task)
* [**StatefulSet**](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/): Workloads that require a readable/writable data volume (hard disk) that persists beyond restarts and is not shared amongst other instances of the same application. Use cases include services that implement eventual consistency (eg. transactional databases)
* [**DaemonSet**](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/): Workloads that should be distributed across all targeted Nodes (eg. log collectors, system monitors, security software)

## Configuration <a href="3ca0" id="3ca0"></a>

So we’ve got a plan, but applications tend to be a fickle bunch and also need to be told certain things at runtime instead of plan-time; Things such as which network interface to bind to, which port to listen on, which API keys to use _et cetera_. While these things can technically be hard-coded, best-practices of 2020 generally suggest they should not. So we need a way to **configure** our application at runtime.

* [**ConfigMap**](https://kubernetes.io/docs/concepts/configuration/configmap/): Stores configuration for mounting as environment variables or read-only files (eg. `SERVER_PORT=8080`)
* [**Secret**](https://kubernetes.io/docs/concepts/configuration/secret/): Fundamentally the same but stored in Base64 encoded text and used for values that should not be stored in plaintext (eg. `GITHUB_API_TOKEN=😱`)

## Persistence <a href="3256" id="3256"></a>

Now what if our application handles files and requires a hard disk that it can read and write from across different versions and instances? We need our data to **persist** by providing our application’s system with a hard disk (AKA volume in technical terms).

* [**PersistentVolume**](https://kubernetes.io/docs/concepts/storage/persistent-volumes/): A logical “hard drive” (eg. Seagate 2TB, EBS)
* [**PersistentVolumeClaim**](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim): Binds a PersistentVolume to a Pod. This is more of a virtual construct: think of it as the act of mounting a hard drive. A PVC defines that intention to mount a hard drive and expose its filesystem for an application to use.

## Execution <a href="25ee" id="25ee"></a>

So we’ve defined how our application will be deployed and configured, and we’ve given it some hard disk space to use. All that’s left now is to **execute** the application.

* [**Pod**](https://kubernetes.io/docs/concepts/workloads/pods/): One instance of your application (eg. `npm start`, `go run ./...`)
* [**ReplicaSet**](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/): Maintains the desired count of your application instances.

## Access Control <a href="cc1b" id="cc1b"></a>

But what if someone accidentally introduced a nasty virus into our application (somehow)? In an enterprise environment with compliance teams nagging at the whole DevOps thingy, we need to ensure that applications can only access resources that it needs to access. We on the other hand, need **access control**.

* [**Namespace**](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/?spm=a2c65.11461447.0.0.15d87863YiFv0v): Defines a virtual boundary within a cluster for access control mechanisms to be implemented on top of. Think about these like browser tabs: an open Facebook tab shouldn’t know what you’re googling in another tab.
* [**ServiceAccount**](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/): Defines a virtual user that can be assigned a Role or ClusterRole which contains a set of permissions scoped to a set of resources (eg. your user login on your machine)
* [**Role**](https://kubernetes.io/docs/reference/access-authn-authz/rbac/): Defines namespace-scoped resource access permissions. Linked to a ServiceAccount with namespace-scoped access via a [**RoleBinding**](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#rolebinding-v1-rbac-authorization-k8s-io) resource
* [**ClusterRole**](https://kubernetes.io/docs/reference/access-authn-authz/rbac/): Defines cluster-wide resource access permissions. Linked to a ServiceAccount with cluster-wide access via a [**ClusterRoleBinding**](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#clusterrolebinding-v1beta1-rbac-authorization-k8s-io) resource

## Exposure <a href="a07e" id="a07e"></a>

Finally, there’s no use of an application that no one can access. After we’ve made the application happy and it’s running as expected, we **expose** it to the rest of the network/public internet for users and other services to access it.

* [**Service**](https://kubernetes.io/docs/concepts/services-networking/service/): Exposes a workload to the cluster. Think running `npm start` on a server application and your application being available on `localhost:3000`. Behind the scenes, your application is binding to a port on your computer. A Service defines that binding.
* [**Ingress**](https://kubernetes.io/docs/concepts/services-networking/ingress/): Exposes a Service to outside the cluster. Ever tried asking your co-worker to access your `localhost:3000`? Chances are you’d have ended up using `ngrok` or a similar tunneling software. An Ingress is basically an `ngrok` that exposes your `localhost:3000` so it becomes accessible to a larger network.

## Linking It All Up —Some Common Deployment Patterns <a href="5980" id="5980"></a>

They said a picture is worth a thousand words. So here’s some diagrams to visualise how all the above links up.

### An HTTP-based API server <a href="c9c3" id="c9c3"></a>

![](<../../.gitbook/assets/image (1).png>)

### A periodic job <a href="c929" id="c929"></a>

![](https://miro.medium.com/max/1920/1\*nk13i5v5HNA4E5ExyFjfMg.png)

### An eventually consistent database system <a href="e477" id="e477"></a>

![](https://miro.medium.com/max/1920/1\*eVv5fF4PTCjbfb29lozPHw.png)

### A cloud-monitoring agent <a href="2ca6" id="2ca6"></a>

![Image for post](https://miro.medium.com/max/60/1\*JFGyL0PMaKlD4MPpDiPfpw.png?q=20)![Image for post](https://miro.medium.com/max/1920/1\*JFGyL0PMaKlD4MPpDiPfpw.png)
