**NOTE**

We are assuming the cluster that you are going to use to go through the tasks that are mentioned, is setup using Kind ([kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)).

_Timelines_: ~8 days

**Objective**

Go through the links mentioned against each category and you should have a fair understanding of the points mentioned for the category.

## What is Kubernetes

- [ ]  [Basic understanding](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

## [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

- [ ] Kube-API Server

- [ ] Kube Controller Manager

- [ ] Kube Scheduler

- [ ] Kubelet

- [ ] Kube Proxy

### Tasks:

- [ ] Try to figure out all the flags the core components (API Server, Controller Manager, Scheduler) are running with.

- [ ] What are all the controllers that are are enabled by default in Controller manager component?

## [Workloads](https://kubernetes.io/docs/concepts/workloads/)

- [ ] Pods

  - InitContainers
  - Probes
  - Pod LifeCycle States

- [ ] Deployments

- [ ] ReplicaSet

- [ ] StatefulSets

- [ ] DaemonSet

- [ ] Deployment strategy types (rolling, re-create etc.)

### Tasks

- [ ] Run a pod that can be used to run network utility (`curl`, `dig`, `nslookup`) commands against an endpoint. ex, run - `curl https://www.google.com`
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/workloads/networkutils.yaml

- [ ] Deploy a Pod which serves a static file. The static file should be created when the Pod starts (init container). The pod is able to serve traffic only if the file exists. Gets restarted if the file is deleted. Also, configure liveness and readiness probes
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/workloads/initcontainer.yaml

- [ ] Create an nginx pod on every node of Kubernetes cluster, if a new node added to the cluster this pod should automatically be added on that node as well.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/workloads/autoscalingpod.yaml

- [ ] Run an nginx pod, upgrade the image tag to (`latest-not-available`) that would break the pod. Now rollback that upgrade to revert the changes. Hint: Use rollout to see and revert the changes.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/workloads/correctnginxdeployment.yaml 
Then for updating the image version `kubectl set image deployment.v1.apps/nginx-deploy nginx=nginx:latest-not-available` and for rolling back use `kubectl rollout undo deployment nginx-deploy`

- [ ] Run a postgres stateful application on the cluster.
- Config yamls: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/workloads/postgres


## [Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/)

- [ ] Kubernetes Objects and its management

- [ ] Namespaces

- [ ] Labels and Selectors

- [ ] Annotations

- [ ] Field Selectors

### Tasks

- [ ] Change the kube-scheduler pod to point to image tag (`:not-available`) from kube-system namespace and then create any pod, try to understand why that pod is not in running state. You might need to read about static pod about this.
- This is because kube-scheduler pod in kube-system namespace of a kuberntes cluster is responsible for scheduling a pod in the cluster. And since here we have updated the image tag of the kube-scheduler pod to an non working image, the pod does not work as expected and therefore when new pods are created in the cluster, they remain in pending state.

- [ ] Create two replicas of a pod using deployment, then manually change the labels of the pods to observe new pod being spinned up. Please make sure you understand what's happening.
- When we manually change the labels of the pods made from deployment X, the controller-manager observes that there are no pods created for deployment X instead of 2. Controller-manager thinks of the updated pods as individual pods, since their label is changed, which is what makes deployment and underlying pods connected. Therefore, 2 new pods are created for deployment X.s


## [Networking](https://kubernetes.io/docs/concepts/services-networking/)

- [ ] Service (How services communicate)

- [ ] [Network Policies](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/)


### Tasks

- [ ] Create an nginx deployment, expose it using service and then run another pod in the same namespace to access that nginx service.
- Config yamls: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/networking/one


- [ ] Create an nginx deployment (in namespace `ns-one`), expose it using service and then run another pod in another namespace (`ns-two`) to access that nginx service.
- Config yamls: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/networking/two

- [ ] Create a network policy that is not going to allow the connection that we mentioned in point 2.
- Config yamls: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/networking/three


## [Configuration](https://kubernetes.io/docs/concepts/configuration/)

- [ ] ConfigMaps

- [ ] Secrets

- [ ] Managing Resources for Containers

- [ ] Environment Variables


### Tasks

- [ ] Create a configmap `cm-one` with key `name` and set environment variable `envname` in a pod with value to the value that is set in configmap (`cm-one`) for key `name`.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/configuration/one

- [ ] Create a secret and set all the key value pairs of the secret as environment variable into a pod.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/configuration/two

- [ ] Create a secret from literal and from file to check if in which cases values are encoded and in which case they are not.
Ans. Values are encoded in both cases.

## [Storage](https://kubernetes.io/docs/concepts/storage/)

- [ ] Volumes

- [ ] Persistent Volumes

- [ ] Storage Classes

- [ ] Dynamic Volume Provisioning

### Tasks

- [ ] Understand what is dynamic volume provisioning

- [ ] Deploy a pod with two containers, one container should write some data (date let's say) to the volume and another container should read that data from volume and display it.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/storage/one.yaml

- [ ] Create a pod that uses hostPath as volume and understand the drawbacks of it.
- Config yaml: https://github.com/NikhilSharmaWe/kubernetesmodule/blob/kubernetes-module-tasks/configurationyamls/storage/two.yaml
Drawbacks of using hostPath volume is the application can make changes in the files present in the host directly.

## [Security](https://kubernetes.io/docs/concepts/security/)

- [ ] [RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

- [ ] [Authentication](https://kubernetes.io/docs/concepts/security/controlling-access/)

### Tasks

- [ ] Deploy a pod (`kanisterio/kanister-kubectl:1.18`) in default namespace and from that pod you should be able to list all the pods from `kube-system` namespace.
- Config yamls: https://github.com/NikhilSharmaWe/kubernetesmodule/tree/kubernetes-module-tasks/configurationyamls/security

- [ ] Which flag in the API Server specifies the authorization mechanism for your kubernetes cluster.
- Flag `--authorization-mode` specifies the authorization mechanism for a kubernetes cluster, we can see that in the config yaml for api-server pod in the kube-system namespace of a kubernetes cluster.

- [ ] What is default service account and where/how it is mounted on a pod in a namespace.
- Default ServceAccount is the default ServiceAccount which is created for every namespace when it is created. This default ServiceAcount gets assigned to all the pods in the namespace by default. This ServiceAccount only have authorization to list all the api-resources of the cluster.

## Running workloads to completion

- [ ] [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

- [ ] [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)


## Final Task

In the Go Programming module we created an application that has two components (API and database), deploy that application on Kubernetes using the concepts that
we discussed above.
Create manifests for all the resources that are involved and then actually deploy that application to make sure things are working as expected.

- Code and manifest yamls for the golang library server: https://github.com/NikhilSharmaWe/golibrary

## Advanced Topics

- [ ] Volume Snapshots

- [ ] Volume Snapshot Classes

- [ ] Volume Snapshot Content
