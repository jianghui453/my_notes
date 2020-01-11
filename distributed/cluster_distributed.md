# differences between cluster and distributed

## Cluster

### Two key characteristics

Cluster is a group of service entities that work together to provide a more scalable and usable service platform than a single service entity.

In the client,a cluster is like a service entity, but in fact, a cluster is made up of a set of service entities. Compared with a single service entity, the cluster provides the following two key features:

- Scalability - cluster performance is not limited to a single service entity, new service entities can be dynamically added to the cluster, thereby enhancing the performance of the cluster.

- High availability - clusters enable clients to avoid the warnings of out of service easily through service entity redundancy. In a cluster, the same service can be provided by multiple service entities. If a service entity fails, another service entity will take over the failed service entity. The functionality provided by a cluster to restore an error from a service entity to another service entity enhances the usability of the application.

### Two abilities

In order to have scalability and high availability, the cluster must have the following two abilities:

- Load balancing -- load balancing can distribute tasks evenly to the computing and network resources in cluster environments.

- Error recovery - for some reason, the resources that perform a task fail, and the resources in another service entity perform the same task and then complete the task. Because of the fact that the resources in one entity cannot work, the process of continuing the tasks transparently in another entity is called error recovery.

### Two major technologies

There are two technologies needed to implement cluster services:

- Cluster address-Clusters are composed of multiple service entities, and cluster clients acquire the functions of each service entity within the cluster by accessing the cluster address of cluster. Having a single cluster address is a basic feature of a cluster.The setting of maintaining cluster address is called load balancer. The load balancer is responsible for managing the entry and exit of each service entity, and is responsible for the conversion of cluster address to the address of the internal service entity. Some load balancers implement real load balancing algorithms, while others only support task conversion. Only the realization of task switching load balancer suitable for cluster environment, support ACTIVE-STANDBY where only one service entity in the cluster, when the working entity fails, the load balancer to the subsequent tasks to another service entity.

- Internal communications -In order to work together, achieve load balancing and error recovery, the cluster entities must communicate frequently, such as load balancer heartbeat test information on the service entity, Communication of task execution context information among service entities.

With the same cluster address, the client can access the computing services provided by the cluster,A cluster address hides the internal address of each service entity, so that the computing services required by the client can be distributed among the various service entities. Internal communication is the basis for the cluster to work properly, It enables the cluster to have balanced load and error recovery capability.

## Distributed

Distributed refers to deploying different service modules in many different servers, and providing services externally through remote invoking collaborative work.

Distributed applications and services

Layering and segmenting applications and services, Then the application and service modules are deployed in a distributed way.

It can not only improve concurrent access ability, but also reduce database connection and resource consumption, It also enables different applications to reuse common services, making the business easy to extend.

## Difference

The distribution is in parallel, and the clusters are connected in series.

1)Distributed refers to the distribution of different businesses in different places. And cluster refers to the combination of several servers, to achieve the same business.

Each node in the distribution can be clustered. Clusters don't necessarily have to be distributed.

For example, Facebook, access to more people, it can do a cluster. Put a in front of a response server, and end several servers to complete the same business,If there is a business access, the response server to see which server load is not very heavy, which will be given to complete.

But distributed, It`s organization is loose, unlike clusters, there is an organization, one server collapsed, other servers can top up. Each node of the distribution has completed different business, one node collapsed, which is not accessible to the business.

2)Distributed is to shorten the execution time of a single task to improve efficiency, And clusters improve efficiency by increasing the number of tasks executed per unit time.

For example: If a task is composed of 10 subtasks, each subtask needs 1 hours to execute separately, and it takes 10 hours to perform the task on a single server. Using a distributed solution, 10 servers are provided, each server is only responsible for handling a sub task without considering the dependencies between subtasks, and it takes only one hour to complete the task. The cluster scheme also provides 10 servers, each server can handle this task independently. Suppose there are 10 tasks to arrive at the same time, the 10 server will work at the same time,After 1h, 10 tasks finish at same time.It stills finish task in 1hour.

## references

1. [The difference between cluster and distributed](https://www.supinfo.com/articles/single/6495-the-difference-between-cluster-and-distributed)