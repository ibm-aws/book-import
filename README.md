###### RHACM Cluster LifeCycle
In this session, you will explore the Cluster LifeCycle capabilities of Red Hat® Advanced Cluster Management for Kubernetes (RHACM).

###### Goals
- Understand the functions of hub and managed clusters.
- Understand Cluster Lifecycle capabilities.
- Import an OpenShift® cluster to RHACM.

###### Introduction

RHACM allows you to manage many Kubernetes clusters from a single location. While the concept of a stretch cluster is something that is often requested, the complexity and potential problems that come with it due to conditions that are beyond the cluster’s control—such as network latencies—usually outweigh the perceived benefits. The preferred approach is multiple clusters—which can be large or small—that are centrally managed. Red Hat Advanced Cluster Management provides this functionality and more.

Within the RHACM web console, there are four main sections that you use to manage different aspects of your Kubernetes clusters and workloads.


###### Review Architecture
<img src=images/download.png /> <br>

Your hub cluster only does one thing—it runs RHACM. All of the RHACM components that provide functionality for the Cluster Lifecycle, Application Lifecycle, and Governance, Risk, and Compliance (GRC) features run on this cluster. Your hub cluster cannot be managed. If you were to try to add the hub cluster as a managed cluster, you would see an error message and that action would be prevented. Do not attempt this.

Your managed cluster also does one thing—it runs your workloads. Customer application workloads are obviously going to run here, but managed clusters also run the RHACM endpoint components. These endpoint components are deployed as Pods and scheduled to run on nodes in the cluster just as any other application would.

RHACM employs a pull-based architectural model that relies on an agent in the cluster joining an external management plane. This architecture has emerged across the Kubernetes and GitOps ecosystem, and it is where Red Hat has focused its upstream and product investments. The endpoint that you deploy onto the managed cluster communicates with RHACM running on the hub cluster.

While a managed cluster can be either OpenShift or another flavor of Kubernetes, the hub cluster can only be deployed on OpenShift. In this lab, both clusters are deployed in a supported cloud provider. The cloud provider may be different depending on when and where you are doing this lab. This does not really matter because everything in the cloud provider that you need is abstracted for you by OpenShift.

Your clusters are slightly different in size. Each one has three master nodes to provide a highly available control plane. Your hub cluster has three workers, while your managed cluster only has two. This is due to the resources required by the RHACM components and can be further tuned. For instance, by using larger worker instances, the number of worker nodes could be decreased in quantity.

###### Review Cluster LifeCycle functionality


The Cluster Lifecycle feature in RHACM allows you to manage the deployment, deletion, importing, and upgrading of your Kubernetes clusters within the following parameters:

Deploying new OpenShift clusters to supported platforms. This includes Amazon Web Services, Microsoft Azure, and Google Cloud. Bare metal is included as a tech preview feature.

Deleting or destroying an OpenShift cluster that was deployed by RHACM. You cannot destroy a cluster that you imported, but you can remove it from RHACM if you no longer want to manage it.

Importing existing Kubernetes clusters. This is not limited to any specific platforms.

Upgrading managed OpenShift clusters.

<img src=images/rhacm.gif /> <br>

###### Import Cluster to RHACM Console.

<img src=images/clusterimport.png/> <br>

- In the RHACM console, click the Copy icon to copy the command you generated previously into your clipboard:

<img src=images/clustercopy.png/> <br>

- Switch back to the SSH session you opened to the bastion VM of your managed cluster, paste the command you just copied, and press Enter.

  - Expect the result of running that command to match the following:

Sample Output:
  ```
customresourcedefinition.apiextensions.k8s.io/klusterlets.operator.open-cluster-management.io created
clusterrole.rbac.authorization.k8s.io/klusterlet created
clusterrole.rbac.authorization.k8s.io/open-cluster-management:klusterlet-admin-aggregate-clusterrole created
clusterrolebinding.rbac.authorization.k8s.io/klusterlet created
namespace/open-cluster-management-agent created
secret/bootstrap-hub-kubeconfig created
secret/open-cluster-management-image-pull-credentials created
serviceaccount/klusterlet created
deployment.apps/klusterlet created
klusterlet.operator.open-cluster-management.io/klusterlet created  
  ```

###### Import K8s Cluster to RHACM Console through kubeconfig file.

<img src=images/kubeimport.png/> <br>

###### After Import Status of the Cluster.

<img src=images/status.png/> <br>

  - We have noticed 3 clusters in our RHACM Console, Will explain on this demo.


##### Application LifeCycle

- Understand Application Lifecycle capabilities in RHACM

- Deploy an application to a managed cluster

- Remove a deployed application from a managed cluster

###### Introduction
In the above section, we explored the Cluster Lifecycle functionality in RHACM. This allowed you to add an OpenShift® cluster to RHACM, which you can now use to deploy applications.

###### Review Application LifeCycle functionality

Application Lifecycle functionality in RHACM provides the processes that are used to manage application resources on your managed clusters. This allows you to define a single- or multi-cluster application using Kubernetes specifications, but with additional automation of the deployment and lifecycle management of resources to individual clusters.

An application designed to run on a single cluster is straightforward and something you ought to be familiar with from working with OpenShift fundamentals.

A multi-cluster application allows you to orchestrate the deployment of these same resources to multiple clusters, based on a set of rules you define for which clusters run the application components.

###### Deploying Application

Now that your application and all of its associated components are created, you are ready to deploy to a target cluster. But you still need to instruct RHACM where to deploy your application and, more specifically, our **Subscription** resource. The lack of this direction is why our **Subscription** resource is currently in a Failed state.

<img src=images/createapp1.png/> <br>

<img src=images/createapp2.png/> <br>

###### Topology Review

<img src=images/topology.png/> <br>

Launch Route URL:

<img src=images/route.png/> <br>

Sample output:

<img src=images/output.png/> <br>


###### Review of GRC functionality.

To begin, it is important to define exactly what GRC is. In RHACM, you build policies that are applied to managed clusters. These policies can do different things, which are described below, but they ultimately serve to govern the configurations of your clusters. This governance over your cluster configurations reduces risk and ensures compliance with standards defined by stakeholders, which can include security teams and operations teams.

This table describes the three types of policy controllers available in RHACM along with the remediation mode they support:

<img src=images/policy-table.png/><br>

###### Create Policy.
This is a complex topic, and this course is only providing an overview. Please consult the GRC product documentation for more details on any of these policy controllers.


<img src=images/createpolicy.png/> <br>



###### Summary

- We have now learned the overview of the Cluster Lifecycle feature in RHACM.
- We have now learned the overview of the Application LifeCycle feature in RHACM.
- We have now learned the overview of Governance, Risk, and Compliance functionality in RHACM.
