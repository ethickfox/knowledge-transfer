==**Kubernetes**== (also known as k8s or “kube”) is an open source container orchestration platform that automates many of the manual processes involved in deploying, managing, and scaling containerized applications.

**Upsides**
- Self-Healing
- Automated Rollbacks
- Horizontal scaling
- High availability
- Portability - allows to deploy and manage apps on different envs and clouds
- Disaster recovery - backup and restore
**Downsides**
- Complexity of distributed systems - complex to setup and operate
- Highter resources consumption
**With Kubernetes you can:**
- Orchestrate containers across multiple hosts.
- Make better use of hardware to maximize resources needed to run your enterprise apps.
- Control and automate application deployments and updates.
- Mount and add storage to run stateful apps.
- Scale containerized applications and their resources on the fly.
- Declaratively manage services, which guarantees the deployed applications are always running the way you intended them to run.
- Health-check and self-heal your apps with autoplacement, autorestart, autoreplication, and autoscaling.

![[Untitled 120.png|Untitled 120.png]]
### Main parts
==**Cluster**== - is a grouping of nodes that run containerized apps in an efficient, automated, distributed, and scalable manner. A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

==**Control plane**== **-** the collection of processes that control Kubernetes nodes. This is where all task assignments originate.
- ==**kube-apiserver (api)**== - is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the external interface (synonym = front end) for the Kubernetes control plane.
- ==**etcd**== - is a consistent (stable) and highly-available key value store used as Kubernetes' backing store for all cluster data and used by other control pane copmponents ([link](https://www.ibm.com/topics/etcd) how it works).
- ==**kube-scheduler**== - is a control plane component that watches for newly created pods with no assigned node, and selects a node for them to run on. 
	Factors taken into account for scheduling decisions include: 
	- individual and collective resource requirements, 
	- hardware/software/policy constraints, 
	- affinity (closeness) and anti-affinity specifications, 
	- data locality, 
	- inter-workload interference
	- deadlines.
- ==**kube-controller-manager (c-m)**== - is a control plane component that runs controller processes. Controller - a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state.
> [!warn] Example
> **ReplciationController** - checks if there are requred number of replicas
> **DeploymentController** - Rolling updates and rollbacks of deployments
- ==**cloud-controller-manager (c-c-m)**== - is a Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster. The cloud-controller-manager only runs controllers that are specific to your cloud provider. If you are running Kubernetes on your own premises, or in a learning environment inside your own PC, the cluster does not have a cloud controller manager.

**==Nodes==** - these machines perform the requested tasks assigned by the control plane.
- **Worker node** - regular node where containers run
	- **kublet** - deamon that communicates with control plane, starts containers
	- **container runtime** - pulls container from registry, running and stopping them and managing resources
	- **kube proxy** -network proxy that routes traffic from service to pod, provides loadbalancing
- **Master node** - main node which manages worker nodes
	- **Api Server** - cluster gateway
		- allows to interact with k8s cluster
		- acts as gatekeeper for authentication to allow only authorized interacvtions with cluster
	- **Scheduler** - decides on which worker nodes pods will be created 
	- **Controller manager** - detects state changes - like pod's dying
	- **etcd** - key-value store for cluster changes, like pods created etc
**==Pod==** - smallest unit in Kubernetes. A group of one or more containers deployed to a single node. All containers in a pod share an IP address, IPC, hostname, and other resources. Pods abstract network and storage from the underlying container. This lets you move containers around the cluster more easily.
- Abstraction over container - no need to bind to specific container technology
- In most cases only one container inside
- They are ephemeral - could die easily and be recreated
- They are stateless, so all data after restart is gone
**==Network==** - k8s provides a virtual network
- Each pod has it's own internal ip address
- Each time pod is recreated it gets a new address
- 
**==Deployments==** 
- Ensures the desired number of **Pods** are running.
- Provides **rolling updates** and **rollbacks**.
==**StatefulSet**== - like deployment, but supposed to be used by stateful apps(like DB)
- Difficult in configuration
- dbs usually hosted outside k8s
**==Services==** - manages access to pod and it's replicas
- Exposes static ip address for the Pod, so if it's restarted, address won't be change
- Lifecycle of pod and service are not connected
- Serves as Load Balancer
- External - exposes address outside of k8s network
- Internal - address is reachable only in k8s network
**==Ingress==** - forwards requests to the particular k8s service

==**ConfigMap**== - configuration storage for application. Not suitable for passwords etc
==**Secret**== - stores data like ConfigMap, but securely. Could contain passwords
- Stores everything base64 encoded
==**Volume**==  - data storage outside of k8s, attached to cluster
- Local storage
- Remote storage
**Namespaces** - let you isolate and organize your workloads

```YAML
apiVersion: v1
kind: Namespace
metadata:
  name: development
```

![[Untitled 1 41.png|Untitled 1 41.png]]

==**Kubelet**== - this service runs on nodes, reads the container manifests, and ensures the defined containers are started and running.

==**Replication controller**==  - this controls how many identical copies of a pod should be running somewhere on the cluster.

==**Service**== - this decouples work definitions from the pods. Kubernetes service proxies automatically get service requests to the right pod—no matter where it moves in the cluster or even if it’s been replaced.

==**kubectl**== - the command line configuration tool for Kubernetes.

```
kubectl create deployment my-app --image=nginx
kubectl get nodes  # List available nodes
kubectl get deployments  # Check deployments
kubectl get pods  # Check running pods
kubectl expose deployment my-app --type=NodePort --port=80
kubectl scale deployment my-app --replicas=3
kubectl delete deployment my-app
kubectl delete service my-app
```

### Kubernetes Explanation in Pictures

[![](https://lh4.googleusercontent.com/Llf5zOzb4A3B-NdgSM1Y-T58UX3E47QdXeOK2y2DRTBC5-k68a-7tEYmevtfSpHKgHaSkOuHs6Za4-oYs73dwCsbXVpI3wJICPIpoa8rGFkoe5CtNuK1aPS0zkOrAvUNvteYkevsDE74KRY40l83LRNyyWw6DjvJUFxh-Jvf9qSeT24CXl4Jxauld67oKQ)](https://lh4.googleusercontent.com/Llf5zOzb4A3B-NdgSM1Y-T58UX3E47QdXeOK2y2DRTBC5-k68a-7tEYmevtfSpHKgHaSkOuHs6Za4-oYs73dwCsbXVpI3wJICPIpoa8rGFkoe5CtNuK1aPS0zkOrAvUNvteYkevsDE74KRY40l83LRNyyWw6DjvJUFxh-Jvf9qSeT24CXl4Jxauld67oKQ)

[![](https://lh6.googleusercontent.com/lc6_1Ms37nKRGLpII26lezk4NHyAlbTHC-Fqxh3mC5-e3gs_BjAvYmcsbHtMiCTJEx1Chkcqz7bUJ36VJZcIRtuLeukYbfm7zhAmoR2r_JrpXh5d-HOjQTrJ_sauh5FWAxaxqLy6ZjhBMapQAd152Mxn2SvLOBxuXCBwzh9vatL4WHoPGamUmPDMAO--4w)](https://lh6.googleusercontent.com/lc6_1Ms37nKRGLpII26lezk4NHyAlbTHC-Fqxh3mC5-e3gs_BjAvYmcsbHtMiCTJEx1Chkcqz7bUJ36VJZcIRtuLeukYbfm7zhAmoR2r_JrpXh5d-HOjQTrJ_sauh5FWAxaxqLy6ZjhBMapQAd152Mxn2SvLOBxuXCBwzh9vatL4WHoPGamUmPDMAO--4w)

[![](https://lh4.googleusercontent.com/yALKKPP8Rl4QhTAAmCumvuD5fwPfzVXEUIMrp2IWMohyRYBmFAnO0zoPDfhhIhZCWc3fP73-P2erG6landIn6Nuhw36ESYF0Lho45ang99SHfJcR9fq-MROLkrT76-BL2G7nWe3V2DN3f9L62Dbh_BUtB6RX4TLl1AZ1lWFoW6KqwCQ1Bf_I7kZBlXrgCg)](https://lh4.googleusercontent.com/yALKKPP8Rl4QhTAAmCumvuD5fwPfzVXEUIMrp2IWMohyRYBmFAnO0zoPDfhhIhZCWc3fP73-P2erG6landIn6Nuhw36ESYF0Lho45ang99SHfJcR9fq-MROLkrT76-BL2G7nWe3V2DN3f9L62Dbh_BUtB6RX4TLl1AZ1lWFoW6KqwCQ1Bf_I7kZBlXrgCg)

### **Node**

[![](https://lh6.googleusercontent.com/nW1kINgSTMx48Uiv3k3P0xZqYxOeSK40vz7WGqFeJFG-sCcB0Oz9F5HGwDOGdsFUN_vKRQo436Wn691ObcrT7jUwxUV_BikIvrXVniDmfs-Lk40gedMvbzmbfLfOJ6QgJlgdUdimA4S7W59t5XXukc_ZKMovR5z_i7j9MNcF3LfBYQkqXpIOvAaTAqjY-A)](https://lh6.googleusercontent.com/nW1kINgSTMx48Uiv3k3P0xZqYxOeSK40vz7WGqFeJFG-sCcB0Oz9F5HGwDOGdsFUN_vKRQo436Wn691ObcrT7jUwxUV_BikIvrXVniDmfs-Lk40gedMvbzmbfLfOJ6QgJlgdUdimA4S7W59t5XXukc_ZKMovR5z_i7j9MNcF3LfBYQkqXpIOvAaTAqjY-A)

it’s a virtual or physical machine

Usually 1 Application per Pod

Each Pod gets its own IP address

Pods are ephemeral

  

[![](https://lh5.googleusercontent.com/kSl-OFfhMfxWOuJQTOjFuzomwcZlSme34b3CGDZiUZhqMQrhGCq77UVxuzMFinGPFxeu96QkSONomj9LyR0lVequQ5VxfhBnUin7NjYOEh1qy4SCPKTypVdv0-jehtyFGIq8a2ElRyvBgbUKWt666CeMgMcr3FrAAcnAvQD1ZyWimJyYNSr7rQQ3TngU0Q)](https://lh5.googleusercontent.com/kSl-OFfhMfxWOuJQTOjFuzomwcZlSme34b3CGDZiUZhqMQrhGCq77UVxuzMFinGPFxeu96QkSONomj9LyR0lVequQ5VxfhBnUin7NjYOEh1qy4SCPKTypVdv0-jehtyFGIq8a2ElRyvBgbUKWt666CeMgMcr3FrAAcnAvQD1ZyWimJyYNSr7rQQ3TngU0Q)

[![](https://lh3.googleusercontent.com/0x2dLAfZXnmTGTcR39Hmj1sRjJn8ezwZjqouZKGd0ShMSQVwTOPu9Gr9SDW-71f99boDq-KekkDQSKfpr32w3c6CmLbfGukzsaEU6hMoVf9NZH09OCTAAkGHHFPhVjnjGWUTyozcCVJOARue9qBdjZv2w58CjyD2NXZy-huRxFXUaUzcNq9RUubwbM0Pxw)](https://lh3.googleusercontent.com/0x2dLAfZXnmTGTcR39Hmj1sRjJn8ezwZjqouZKGd0ShMSQVwTOPu9Gr9SDW-71f99boDq-KekkDQSKfpr32w3c6CmLbfGukzsaEU6hMoVf9NZH09OCTAAkGHHFPhVjnjGWUTyozcCVJOARue9qBdjZv2w58CjyD2NXZy-huRxFXUaUzcNq9RUubwbM0Pxw)

[![](https://lh3.googleusercontent.com/9Ho5vZibHEH8RmIwvpcKLPBvkHia5UfFK2sZ8pvxm82VYdB-iJWwLQ4T_wf4rDTavc4AXPdckEbgPRRKBc18pCtKQ5bDM8f6I_thb0FChKEE0-bulsvS4NeFRUku2xi-mXp1jSC_3DIhpnCY-MTCShCntZrMop9jf8COmLAxUnC5qbwmCGNWTegbkASdiA)](https://lh3.googleusercontent.com/9Ho5vZibHEH8RmIwvpcKLPBvkHia5UfFK2sZ8pvxm82VYdB-iJWwLQ4T_wf4rDTavc4AXPdckEbgPRRKBc18pCtKQ5bDM8f6I_thb0FChKEE0-bulsvS4NeFRUku2xi-mXp1jSC_3DIhpnCY-MTCShCntZrMop9jf8COmLAxUnC5qbwmCGNWTegbkASdiA)

[![](https://lh4.googleusercontent.com/41h9azF_0mA7l_mkUz9o8CNSs0NulPQPlDZMLjg-gqS6RF1bYTnxFH3lEimAU5zTUib0MmmJ0IynumLoiiLQCHdbPYBaIG3_f4Vmox3Iby7ck676dy4nLNQPFfCd_0BKiAmzetLP_4Lrb8FczYtSYChuE9PnhKLm4L7c-XexdlGc3d8I3eushWe8wkRMww)](https://lh4.googleusercontent.com/41h9azF_0mA7l_mkUz9o8CNSs0NulPQPlDZMLjg-gqS6RF1bYTnxFH3lEimAU5zTUib0MmmJ0IynumLoiiLQCHdbPYBaIG3_f4Vmox3Iby7ck676dy4nLNQPFfCd_0BKiAmzetLP_4Lrb8FczYtSYChuE9PnhKLm4L7c-XexdlGc3d8I3eushWe8wkRMww)

[![](https://lh6.googleusercontent.com/SwtIr1bKgj-r3POZ3ryImGRtq8AM5sOLXWpLZHbPwpz2oJDM9HLPoIJv_5eR10PV_T9w270MNrHA8xQfiCm4ZpeAr2jAxQpgZXW_Ppn0PV5UZfbTFiZi-qrZJq3Bqi_bVzDiyBhwOvqBgyVNKB7yWxIBZ7ApiuIMKAuVXasT0DYmjl-14jHVuifUQBTnKw)](https://lh6.googleusercontent.com/SwtIr1bKgj-r3POZ3ryImGRtq8AM5sOLXWpLZHbPwpz2oJDM9HLPoIJv_5eR10PV_T9w270MNrHA8xQfiCm4ZpeAr2jAxQpgZXW_Ppn0PV5UZfbTFiZi-qrZJq3Bqi_bVzDiyBhwOvqBgyVNKB7yWxIBZ7ApiuIMKAuVXasT0DYmjl-14jHVuifUQBTnKw)

[![](https://lh3.googleusercontent.com/UMYYrMdYPMfc5M6mrCETKs8WteEaKQhOe3ZRRUiF0gthPHIAX4N_rynj-p5fw7g9E6Atr1-VUbc0-azMhilVcrhIToXQpSObNhtwRNmqRs9QngEx2OjzZReDowTuNptquL1wCbBvmXblROo3_9NocAJz5nDs7cwYVXzbFsYlzJbZ9ftWtsV8Nbhlbjchug)](https://lh3.googleusercontent.com/UMYYrMdYPMfc5M6mrCETKs8WteEaKQhOe3ZRRUiF0gthPHIAX4N_rynj-p5fw7g9E6Atr1-VUbc0-azMhilVcrhIToXQpSObNhtwRNmqRs9QngEx2OjzZReDowTuNptquL1wCbBvmXblROo3_9NocAJz5nDs7cwYVXzbFsYlzJbZ9ftWtsV8Nbhlbjchug)

  

[![](https://lh6.googleusercontent.com/AxFYoXu_gGtOWJMX1o_gTxs7TVs90-4628gaMX53xe8zt0vlwbF5AA1XtSM7frlXkkB19NTgTchXESF3DQKaEU3dXccSIg60-xIf7ZLcS3NSTY_BGDPBpZU1sa5eLE4y8QZaHxbEeNLecc3ShyKp25a1vO7iBDBeleT5NUlFz2IeF1yzL_MsT-KqxuckaQ)](https://lh6.googleusercontent.com/AxFYoXu_gGtOWJMX1o_gTxs7TVs90-4628gaMX53xe8zt0vlwbF5AA1XtSM7frlXkkB19NTgTchXESF3DQKaEU3dXccSIg60-xIf7ZLcS3NSTY_BGDPBpZU1sa5eLE4y8QZaHxbEeNLecc3ShyKp25a1vO7iBDBeleT5NUlFz2IeF1yzL_MsT-KqxuckaQ)

[![](https://lh5.googleusercontent.com/j9NXfq2IkuoKNvjpEHXGVUQ3Fvn_kuos-zzF70IzZ2PCkMV2zTgk820pwBCCPwceURYS5Jsdhk6MX-3H7hxML4an6-sm3KArcIskdUcFrOS5zPmorMzq9GhUfQCudm-ljx3niacPXe5jVwHND9t9x_8wG2h-9EiMUjnLLL7S-NV-5Zn17xKbi6PEH9Pf5A)](https://lh5.googleusercontent.com/j9NXfq2IkuoKNvjpEHXGVUQ3Fvn_kuos-zzF70IzZ2PCkMV2zTgk820pwBCCPwceURYS5Jsdhk6MX-3H7hxML4an6-sm3KArcIskdUcFrOS5zPmorMzq9GhUfQCudm-ljx3niacPXe5jVwHND9t9x_8wG2h-9EiMUjnLLL7S-NV-5Zn17xKbi6PEH9Pf5A)

[![](https://lh6.googleusercontent.com/8ZhI4Ud6ISASWMZk6B4Ud_kcTZXxtifqqzArVh6qLgqnhj6AABsj5HO0f9lSqzFTgqmD0jFAe49awyIzFUoJb4bFGo5-alrgYyGc8GL7pyja56Y1F7BdZOOxxTNQ6huLxp-K1WOuFG6dETGhzJA5rnfZhSmibBVSz_CcAn-ACiDNGj4aSjTT7DKw4rOJVg)](https://lh6.googleusercontent.com/8ZhI4Ud6ISASWMZk6B4Ud_kcTZXxtifqqzArVh6qLgqnhj6AABsj5HO0f9lSqzFTgqmD0jFAe49awyIzFUoJb4bFGo5-alrgYyGc8GL7pyja56Y1F7BdZOOxxTNQ6huLxp-K1WOuFG6dETGhzJA5rnfZhSmibBVSz_CcAn-ACiDNGj4aSjTT7DKw4rOJVg)

[![](https://lh4.googleusercontent.com/JanIzK_uFtaMqmi_spN29oPApAsD2-RPAxO3eXIl2sqjUga0miZqEZJLvZBiVcXuRbfD1NZQ7XM9sXVOfS0jlYMVYMWYeH484GtjnHdS3XxiGt9KgCGnEbedAuxIEoQHAucdbNPVgnq991LlI5yOy1qBAfd1x2mcvp1LK2wA_L2_xC2eCiyR0H463iFclg)](https://lh4.googleusercontent.com/JanIzK_uFtaMqmi_spN29oPApAsD2-RPAxO3eXIl2sqjUga0miZqEZJLvZBiVcXuRbfD1NZQ7XM9sXVOfS0jlYMVYMWYeH484GtjnHdS3XxiGt9KgCGnEbedAuxIEoQHAucdbNPVgnq991LlI5yOy1qBAfd1x2mcvp1LK2wA_L2_xC2eCiyR0H463iFclg)

[![](https://lh4.googleusercontent.com/3rIz0aFKgGB6zZmvNcgC0xzfciYmSx8Lh26EyZJOg73UeUm5Im4Os-jUoNsZGhnqyw7TsQ09zp65KqpGgK8hvR3XqKsHma5HNGJjnmpaJbC4L51RK-cTKdwgPqbtAry9tgacfNjR4fpJ2YHDTBSqwR9Cw8x54cYrTWV-xqB47bjBcjfTp_l7tt0N4QNZvw)](https://lh4.googleusercontent.com/3rIz0aFKgGB6zZmvNcgC0xzfciYmSx8Lh26EyZJOg73UeUm5Im4Os-jUoNsZGhnqyw7TsQ09zp65KqpGgK8hvR3XqKsHma5HNGJjnmpaJbC4L51RK-cTKdwgPqbtAry9tgacfNjR4fpJ2YHDTBSqwR9Cw8x54cYrTWV-xqB47bjBcjfTp_l7tt0N4QNZvw)

[![](https://lh4.googleusercontent.com/_ucF9pX7dit-1i5sESfMW22Dv_Rx9hgJrxsVlHUWuuHEtQCIptMwROz4xp4rSEptCdxtoMyN4q6cY8-_cO3qn0cA7WTyPVgniT9t98Bwcvw0C8b9E932tDq0JWBZKgVqsv18sO8lREOURw6_Bj_yyB6cUHXTdVNL1fTkCPsPZZKEwIE3mI6YTQwzlrOfRQ)](https://lh4.googleusercontent.com/_ucF9pX7dit-1i5sESfMW22Dv_Rx9hgJrxsVlHUWuuHEtQCIptMwROz4xp4rSEptCdxtoMyN4q6cY8-_cO3qn0cA7WTyPVgniT9t98Bwcvw0C8b9E932tDq0JWBZKgVqsv18sO8lREOURw6_Bj_yyB6cUHXTdVNL1fTkCPsPZZKEwIE3mI6YTQwzlrOfRQ)

[![](https://lh3.googleusercontent.com/K0s8s6UdPNhCQSkU6WCG5Tcfcz1VYosAu_de0NXuEHLx6yVpx5W76aAWhcsZeEagit50m8E8LXdUkPLNcDHnTdMVLWFy363JdrLFKzUGtAf5ZT10HH0iUiJ3Gd32TKMySjSL9AllAR-eSIHtQlb15k_hzGOMU9merO2ynKgTs7t0-tkRJfwxxuwoGpPv7Q)](https://lh3.googleusercontent.com/K0s8s6UdPNhCQSkU6WCG5Tcfcz1VYosAu_de0NXuEHLx6yVpx5W76aAWhcsZeEagit50m8E8LXdUkPLNcDHnTdMVLWFy363JdrLFKzUGtAf5ZT10HH0iUiJ3Gd32TKMySjSL9AllAR-eSIHtQlb15k_hzGOMU9merO2ynKgTs7t0-tkRJfwxxuwoGpPv7Q)

[![](https://lh4.googleusercontent.com/C7CxX-mbIk6xZBVZ6W7jjf-k4hlWs3NcKXx5Cgm6v8Lm4KaGIo8dX5aEZtLimjyzRLzxn2YCC75_INKERAz0zcgBW-orhThelAvjoWkXrOsgSlXIholzKmIxpMLO9AOuK4KTGZOJyPGNYXYomSxj5oIYgQMNE4mskssKjVnH8rp_vidaxUsRdYfntEpvDQ)](https://lh4.googleusercontent.com/C7CxX-mbIk6xZBVZ6W7jjf-k4hlWs3NcKXx5Cgm6v8Lm4KaGIo8dX5aEZtLimjyzRLzxn2YCC75_INKERAz0zcgBW-orhThelAvjoWkXrOsgSlXIholzKmIxpMLO9AOuK4KTGZOJyPGNYXYomSxj5oIYgQMNE4mskssKjVnH8rp_vidaxUsRdYfntEpvDQ)

### One node cluster - minikube

[![](https://lh5.googleusercontent.com/PDPOzVM6php4VD9qpl3LZQu-d1KVpr7o70DBF3XEvuZ-5-giqzqlXW3o3GU3ZlRIc-okNdq5MEhpaebls2wMUnyN2p9Ynhk_twPXS-qL2UERlCCQKh4ml5djOsF4dvqW0mNfxmnKRTeOG_lba-OiwO7vV5nU19ERCSSvC88y9BvUOfAxXK6nYGL5mO8-HA)](https://lh5.googleusercontent.com/PDPOzVM6php4VD9qpl3LZQu-d1KVpr7o70DBF3XEvuZ-5-giqzqlXW3o3GU3ZlRIc-okNdq5MEhpaebls2wMUnyN2p9Ynhk_twPXS-qL2UERlCCQKh4ml5djOsF4dvqW0mNfxmnKRTeOG_lba-OiwO7vV5nU19ERCSSvC88y9BvUOfAxXK6nYGL5mO8-HA)

[![](https://lh4.googleusercontent.com/ZAncqd_lDxMhOM1uvaH7iFV_ANAUA-d3zthyiyFoiqUD-PizjHoPIksphjQEcxuw6lomnD7Dxj7BXQ8IlpDbH73fHmre-QYq1UT0ufR9qAw7QLyyXVtWqhOhRj_ZZAkxmp2GOkWKdWnAfaZKA4YOlgg9Fk2Ss_7q43MHN0YRN9RM7HnC_AqKlmm41-WY0w)](https://lh4.googleusercontent.com/ZAncqd_lDxMhOM1uvaH7iFV_ANAUA-d3zthyiyFoiqUD-PizjHoPIksphjQEcxuw6lomnD7Dxj7BXQ8IlpDbH73fHmre-QYq1UT0ufR9qAw7QLyyXVtWqhOhRj_ZZAkxmp2GOkWKdWnAfaZKA4YOlgg9Fk2Ss_7q43MHN0YRN9RM7HnC_AqKlmm41-WY0w)

### kubectl get node

[![](https://lh6.googleusercontent.com/p3TsOsSF8vCO4krvzH_5TVBBdFi1HckeACjCXee_l7uw13AMcmq-ZC5nSEMpMPavJXPah-G0ff0k31yPKVL-w8_13WGDxoK7zjVgVM11PKgx13xgmWhW7pDxOXbrme6RZGz7RQbBGdhlFb-wubLSXztpSPPn8iafowD7PvuPco07HFcpvbJUryqvMAhHHw)](https://lh6.googleusercontent.com/p3TsOsSF8vCO4krvzH_5TVBBdFi1HckeACjCXee_l7uw13AMcmq-ZC5nSEMpMPavJXPah-G0ff0k31yPKVL-w8_13WGDxoK7zjVgVM11PKgx13xgmWhW7pDxOXbrme6RZGz7RQbBGdhlFb-wubLSXztpSPPn8iafowD7PvuPco07HFcpvbJUryqvMAhHHw)

[![](https://lh6.googleusercontent.com/fCuVkIVTEw3rO-Hno-ylUtA5vrM2_nhiBTVlzPyqJ9DWu6e0UsIYSUZZga13wVQjPHI5wNrkl_NBGFqi-ByWwiBOI4ayWzQC-c2PgbyaPyuR-cp1syqMbJBuYDqp8rSQ5WLxokKDsRJqQW0avc0k20namFltulc2QEEFfQqHauwTJD6DqcT4JOcrLCz3rQ)](https://lh6.googleusercontent.com/fCuVkIVTEw3rO-Hno-ylUtA5vrM2_nhiBTVlzPyqJ9DWu6e0UsIYSUZZga13wVQjPHI5wNrkl_NBGFqi-ByWwiBOI4ayWzQC-c2PgbyaPyuR-cp1syqMbJBuYDqp8rSQ5WLxokKDsRJqQW0avc0k20namFltulc2QEEFfQqHauwTJD6DqcT4JOcrLCz3rQ)

[![](https://lh3.googleusercontent.com/c3yf-QP5UgLFUvQVqaUxl7vztOdNbidgRkIidzgIMX045cCdUU2yjxemg3T7BL24kA_JuVuhNeHFdpEpZjoVCLiEFtG_GwYneprCvwtmRr5ZAI3F190YywYJbbO-8pJqACkFplbAfIxRTpn-maN-rXZ9UmPBmB21oEQqxlIA9R_jNwex4X1Hp8QZIRkV5w)](https://lh3.googleusercontent.com/c3yf-QP5UgLFUvQVqaUxl7vztOdNbidgRkIidzgIMX045cCdUU2yjxemg3T7BL24kA_JuVuhNeHFdpEpZjoVCLiEFtG_GwYneprCvwtmRr5ZAI3F190YywYJbbO-8pJqACkFplbAfIxRTpn-maN-rXZ9UmPBmB21oEQqxlIA9R_jNwex4X1Hp8QZIRkV5w)

[![](https://lh3.googleusercontent.com/VA5RhQ_Lzz6bnZCPwFt_-L0yT5v_0NJavz-ubUOpVcuoOiqMdYqkVtW-RauCgF6u8p3iKaQp5dkdCbIE-vPHEhCPaisDWqUTrcVyVDM75AFieBQyW9d42z5Vhuwlyxy_npNq5r8YyfsaRl-6Gebuc6rml0TKbXXAKYv__bBF26ipAklu6diR6oDrvkOSIA)](https://lh3.googleusercontent.com/VA5RhQ_Lzz6bnZCPwFt_-L0yT5v_0NJavz-ubUOpVcuoOiqMdYqkVtW-RauCgF6u8p3iKaQp5dkdCbIE-vPHEhCPaisDWqUTrcVyVDM75AFieBQyW9d42z5Vhuwlyxy_npNq5r8YyfsaRl-6Gebuc6rml0TKbXXAKYv__bBF26ipAklu6diR6oDrvkOSIA)

[![](https://lh5.googleusercontent.com/PeQhUBe4WLk41GsOV0qbRbZjPCtCwIEuvguyPy9wUc14OpWf98FzOroVSvc1TVPPsKQyWz10TgJ5DdigiIqoTM8ybxJIWllYiFtGHIJfTMay7Kh6wZAf_-jiRF8bHrVk3AUVD_nEHOxg2ZcY_BzV3dMngB_fyskwOCTgsD-SJ6HjZm2VF7ppsq7MNfJWcQ)](https://lh5.googleusercontent.com/PeQhUBe4WLk41GsOV0qbRbZjPCtCwIEuvguyPy9wUc14OpWf98FzOroVSvc1TVPPsKQyWz10TgJ5DdigiIqoTM8ybxJIWllYiFtGHIJfTMay7Kh6wZAf_-jiRF8bHrVk3AUVD_nEHOxg2ZcY_BzV3dMngB_fyskwOCTgsD-SJ6HjZm2VF7ppsq7MNfJWcQ)

```YAML
apiVersion: v1
kind: ConfigMap 
metadata:
    name: mongo-config 
data:
    mongo-url: mongo-service


apiVersion: v1
kind: Secret
metadata:
    name: mongo-secret
type: 0paque
data:
    mongo_user: YWRtaW4=
    mongo_password: MwsfsZDFUMmUZN2Rm

apiversion:v1
kind: Deployment
metadata:
    name: mongo-deployment
    labels:
        app: mongo
spec:
    replicas: 1 
    selector:
        matchLabels:
            app: mongo
    template:
        metadata:
            labels:
                app: mongo
        spec:
            containers:
                - name: mongodb 
                image: mongo:5.0
                ports:
                      - containerPort: 27017
			   env:
				﻿﻿	 - name: MONGO_INITDB_ROOT_USERNAME
				﻿﻿	   valueFrom:
    secretKeyRef: 
name: mongo-secret
key: mongo-user
				﻿﻿	 - name: MONGO_INITDB_ROOT_PASSWORD
				﻿﻿	   valueFrom:
    secretKeyRef: 
name: mongo-secret
key: mongo-password
```

```YAML
apiVersion: v1
kind: Service
metadata:
    name: mongo-service
spec:
    selector:
        app: mongo
ports:
    - protocol: TCP
    port: 8080
    targetPort: 27017



apiversion:v1
kind: Deployment
metadata:
    name: webapp-deployment
    labels:
        app: webapp
spec:
    replicas: 1 
    selector:
        matchLabels:
            app: webapp
    template:
        metadata:
            labels:
                app: webapp
        spec:
            containers:
                - name: webapp 
                image: nanajanashia/k8s-demo-app: v1.0
                ports:
                      - containerPort: 3000
			   env:
				﻿﻿	 - name: USER_NAME
				﻿﻿	   valueFrom:
    secretKeyRef: 
name: mongo-secret
key: mongo-user
				﻿﻿	 - name: USER_PASSWORD
				﻿﻿	   valueFrom:
    secretKeyRef: 
name: mongo-secret
key: mongo-password
				﻿﻿	 - name: DB_URL
				﻿﻿	   valueFrom:
    							configMapKeyRef: 
name: mongo-config
key: mongo-url
```

```YAML
apiVersion: v1
kind: Service
metadata:
    name: webapp-service
spec:
	type:NodePort
    selector:
        app: webapp
ports:
    - protocol: TCP
    port: 80
    targetPort: 3000
	nodePort: 30100
```

```Shell
kubectl apply -f mongo-config.yaml
kubectl get all
kubectl get configmap
kubectl get secret
kubectl logs webapp-deployment-deffd6bcc-gsmlt
kubectl get node -o wide
minikube ip
```

## Commands Examples

```Bash
kubectl cluster-info

kubectl get nodes
kubectl get namespaces
kubectl get pods -A      # in all namespaces
kubectl get pods -n development # in namespace development
kubectl get services -A
kubectl get deployments -n development

kubectl apply -f kub.yml

kubectl delete -f kub.yml
kubectl delete pod pod-info-development-78bbb77995-4mdf8 -n development

kubectl logs pod-info-development-78bbb77995-l47xp -n development
```


### Managing Kubernetes for development
```Shell
minikube start
kubectl get nodes  # Verify cluster is running
```
- [Spring Boot with Kubernetes](Spring%20Boot%20with%20Kubernetes.md)
