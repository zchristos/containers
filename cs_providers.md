---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, classic

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}
 


# Supported infrastructure providers
{: #infrastructure_providers}

With {{site.data.keyword.containerlong}}, you can create a cluster from the following infrastructure providers. All the worker nodes in a cluster must be from the same provider. Originally, {{site.data.keyword.containerlong_notm}} provisioned your worker nodes in a single provider, classic infrastructure.
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **Classic**: Create your cluster on a classic compute, networking, and storage environment in IBM Cloud infrastructure.
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **Generation 2 compute**: Create your cluster on the next generation of IBM Cloud infrastructure virtual servers, available as of 20 May 2020.

As of 26 February 2021, all service for VPC Gen 1 is ended. You can now [create new VPC clusters on Generation 2 compute only](/docs/containers?topic=containers-clusters#clusters_vpcg2). [Move any remaining workloads from VPC Gen 1 clusters to VPC Gen 2 clusters](/docs/containers?topic=containers-vpc_migrate_tutorial) before 01 March 2021, when any remaining VPC Gen 1 worker nodes are automatically deleted.
{: deprecated}

|Area|Classic|VPC|
|----|----|----|
|Overview|Clusters on classic infrastructure include all of the {{site.data.keyword.containerlong_notm}} mature and robust features for compute, networking, and storage. To get started, create a [Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) or [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-openshift_tutorial) cluster.|With VPC, you can create your cluster in the next generation of the {{site.data.keyword.cloud_notm}} platform, in your [Virtual Private Cloud](/docs/vpc?topic=vpc-about-vpc). VPC gives you the security of a private cloud environment with the dynamic scalability of a public cloud, with the ability to deploy your cluster worker nodes as generation 2 compute virtual servers. To get started, try out the [VPC Gen 2 compute cluster tutorial](/docs/containers?topic=containers-vpc_ks_tutorial).|
|Doc icons|<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="25" style="width:25px; border-style: none"/> Throughout the documentation, pages and topics might include a note that the content applies only to the classic infrastructure provider.|<img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="25" style="width:25px; border-style: none"/> Throughout the documentation, pages and topics might include a note that the content applies only to the VPC infrastructure provider.|
|Compute and worker node resources|[Virtual](/docs/containers?topic=containers-planning_worker_nodes#vm), [bare metal](/docs/containers?topic=containers-planning_worker_nodes#bm), and [software-defined storage](/docs/containers?topic=containers-planning_worker_nodes#sds) machines are available for your worker nodes. Your worker node instances reside in your IBM Cloud infrastructure account, but you can manage them through {{site.data.keyword.containerlong_notm}}. You own the worker node instances.|VPC supports [only a select group of virtual machines](/docs/containers?topic=containers-planning_worker_nodes#vm) for your worker nodes. Unlike classic clusters, your VPC cluster worker nodes do not appear in your infrastructure portal or a separate infrastructure bill. Instead, you manage all maintenance and billing activity for the worker nodes through {{site.data.keyword.containerlong_notm}}. Your worker node instances are connected to certain VPC instances that do reside in your infrastructure account, such as the VPC subnet or storage volumes.|
|Security|Classic clusters have many built-in security features that help you protect your cluster infrastructure, isolate resources, and ensure security compliance. For more information, see the [classic Network Infrastructure documentation](/docs/cloud-infrastructure?topic=cloud-infrastructure-compare-infrastructure).|With VPC, your cluster runs in an isolated environment in the public cloud. Network access control lists protect the subnets that provide the floating IPs for your worker nodes. For more information, see the [VPC documentation](/docs/vpc?topic=vpc-about-vpc).|
|High availability|For both classic and VPC clusters, the master includes three replicas for high availability. Further, if you create your cluster in a multizone metro, the master replicas are spread across zones and you can also spread your worker pools across zones. For more information, see [High availability for {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha).|Same as Classic; see [High availability for {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha).|
|Reservations | [Create a reservation](/docs/containers?topic=containers-reservations) with contracts for 1 or 3 year terms for classic worker nodes to lock in a reduced cost for the life of the contract. Typical savings range between 30-50% compared to on-demand worker node costs. | Not available. |
|Cluster administration|Classic clusters support the entire set of v1 API operations, such as resizing worker pools, reloading worker nodes, and updating masters and worker nodes across major, minor, and patch versions. When you delete a cluster, you can choose to remove any attached subnet or storage instances.|VPC clusters cannot be reloaded or updated. Instead, use the [`worker replace --update` CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cli_worker_replace) or [API operation](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/replaceWorker){: external} to replace worker nodes that are outdated or in a troubled state.|
|Cluster networking|Your worker nodes are provisioned on private VLANs that provide private IP addresses to communicate on the private IBM Cloud infrastructure network. For communication on the public network, you can also provision the worker nodes on a public VLAN. Communication to the cluster master can be on the public or private cloud service endpoint. For more information, see [Understanding cluster network basics](/docs/containers?topic=containers-plan_clusters#plan_basics).|Unlike classic infrastructure, the worker nodes of your VPC cluster are attached to VPC subnets and assigned private IP addresses. The worker nodes are not connected to the public network, which instead is accessed through a public gateway, floating IP, or VPN gateway. For more information, see [Overview of VPC networking in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-vpc-subnets#vpc_basics).|
|Apps and container platform|You can choose to create [community Kubernetes or {{site.data.keyword.openshiftshort}} clusters](/docs/containers?topic=containers-faqs#container_platforms) to manage your containerized apps. Your app build processes do not differ because of the infrastructure provider, but how you expose the app does, as described in the next _App networking_ entry.|You can choose to create [community Kubernetes or {{site.data.keyword.openshiftshort}} clusters](/docs/containers?topic=containers-faqs#container_platforms) to manage your containerized apps. Your app build processes do not differ because of the infrastructure provider, but how you expose the app does, as described in the next _App networking_ entry.|
|App networking|All pods that are deployed to a worker node are assigned a private IP address in the 172.30.0.0/16 range and are routed between worker nodes on the worker node private IP address of the private VLAN. To expose the app on the public network, your cluster must have worker nodes on the public VLAN. Then, you can create a NodePort, LoadBalancer (NLB), or Ingress (ALB) service. For more information, see [Planning in-cluster and external networking for apps](/docs/containers?topic=containers-cs_network_planning).|All pods that are deployed to a worker node are assigned a private IP address in the 172.30.0.0/16 range and are routed between worker nodes on the worker node private IP address of the private VPC subnet. To expose the app on the public network, you can create a Kubernetes `LoadBalancer` service, which provisions a VPC load balancer and public hostname address for your worker nodes. For more information, see [Exposing apps with VPC load balancers](/docs/containers?topic=containers-vpc-lbaas).|
|Storage|You can choose from non-persistent and persistent storage solutions such as file, block, object, and software-defined storage. For more information, see [Planning highly available persistent storage](/docs/containers?topic=containers-storage_planning).|For persistent storage, use [block](/docs/containers?topic=containers-vpc-block). For the number of volumes that can be attached per worker node, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). The storage class limits the volume size to 20TB and IOPS capacity to 20,000. For non-persistent storage, secondary storage on the local worker node is not available.|
|User access|To create classic infrastructure clusters, you must set up [infrastructure credentials](/docs/containers?topic=containers-users#api_key) for each region and resource group. To let users manage the cluster, use [{{site.data.keyword.cloud_notm}} IAM platform access roles](/docs/containers?topic=containers-access_reference#iam_platform). To grant users access to Kubernetes resources within the cluster, use [{{site.data.keyword.cloud_notm}} IAM service access roles](/docs/containers?topic=containers-access_reference#service), which correspond with Kubernetes RBAC roles.|Unlike for classic infrastructure, with VPC, you can use only [{{site.data.keyword.cloud_notm}} IAM access policies](/docs/vpc?topic=vpc-iam-getting-started) to authorize users to create infrastructure, manage your cluster, and access Kubernetes resources. The cluster can be in a different resource group than the VPC.|
|Integrations|You can extend your cluster and app capabilities with a variety of {{site.data.keyword.cloud_notm}} services, add-ons, and third-party integrations. For a list, see [Supported {{site.data.keyword.cloud_notm}} and third-party integrations](/docs/containers?topic=containers-supported_integrations).|VPC supports a select list of supported {{site.data.keyword.cloud_notm}} services, add-ons, and third-party integrations. For a list, see [Supported {{site.data.keyword.cloud_notm}} and third-party integrations](/docs/containers?topic=containers-supported_integrations).|
|Available locations and versions|Classic clusters are [available worldwide](/docs/containers?topic=containers-regions-and-zones#locations), including all six {{site.data.keyword.cloud_notm}} multizone metros and 20 single zone locations in more than a dozen countries.|VPC clusters are available worldwide in the [multizone location](/docs/containers?topic=containers-regions-and-zones#zones-vpc).|
|Service interface (API, CLI, UI)|Classic clusters are fully supported in the {{site.data.keyword.containershort_notm}} [v1 API ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.cloud.ibm.com/global/swagger-global-api/#/), [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli), and [console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/clusters).|VPC clusters are supported by the [next version (`v2`) of the {{site.data.keyword.containerlong_notm}} API](/docs/containers?topic=containers-cs_api_install), and you can manage your VPC clusters through the same CLI and console as classic clusters.|
|Service compliance|See the classic section in [What standards does the service comply to?](/docs/containers?topic=containers-faqs#standards). | See the VPC section in [What standards does the service comply to?](/docs/containers?topic=containers-faqs#standards). |
|Service limitations|See [Service limitations](/docs/containers?topic=containers-limitations#tech_limits). Feature-specific limitations are documented by section.|See [Service limitations](/docs/containers?topic=containers-limitations#tech_limits).<ul><li>For VPC-specific limitations in {{site.data.keyword.containerlong_notm}}, see [VPC Gen 2 compute cluster limitations](/docs/containers?topic=containers-limitations#ks_vpc_gen2_limits).</li><li>For general VPC infrastructure provider limitations, see [Known limitations](/docs/vpc-on-classic?topic=vpc-on-classic-known-limitations).</li></ul>|
|Troubleshooting and support|Both classic and VPC clusters are supported through the same {{site.data.keyword.cloud_notm}} Support processes. For cluster issues, check out the [Debugging your clusters](/docs/containers?topic=containers-cs_troubleshoot) guide. For questions, try posting in the [Slack channel](https://cloud.ibm.com/kubernetes/slack){: external}.|Both classic and VPC clusters are supported through the same {{site.data.keyword.cloud_notm}} Support processes. For cluster issues, check out the [troubleshooting documentation](/docs/containers?topic=containers-cs_troubleshoot) for VPC-specific topics. For questions, try posting in the [Slack channel](https://cloud.ibm.com/kubernetes/slack){: external}.|
{: caption="Infrastructure providers for {{site.data.keyword.containerlong_notm}} clusters"}
{: summary="The rows are read from left to right, with the area of comparison in column one, classic infrastructure provider in column two, and VPC infrastructure provider in column three."}


