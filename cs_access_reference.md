---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, infrastructure, rbac, policy

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
 


# User access permissions
{: #access_reference}

When you [assign cluster permissions](/docs/containers?topic=containers-users), it can be hard to judge which role you need to assign to a user. Use the tables in the following sections to determine the minimum level of permissions that are required to perform common tasks in {{site.data.keyword.containerlong}}.
{: shortdesc}

## Permissions to create a cluster
{: #cluster_create_permissions}

Review the minimum permissions in {{site.data.keyword.cloud_notm}} IAM that the account owner must set up so that users can create clusters in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

<dl>
<dt>API key for each region and resource group</dt>
<dd>The API key is used to provide the underlying access to infrastructure and other account resources. For more information, see [Setting up the API key to enable access to the infrastructure portfolio](/docs/containers?topic=containers-users#api_key).
<ul><li>**IAM Services**</li><ul>
  <li>**Administrator** platform access role for **Kubernetes Service** in the console (**containers-kubernetes** in the API or CLI) in **All resource groups**.</li>
  <li>**Writer** or **Manager** service access role for **Kubernetes Service** in the console (**containers-kubernetes** in the API or CLI) in **All resource groups**.</li>
  <li>**Administrator** platform access role for **Container Registry** in the console (**container-registry** in the API or CLI) at the **Account** level. Do not limit policies for {{site.data.keyword.registrylong_notm}} to the resource group level.</li>
  <li>If you plan to [expose apps with Ingress](/docs/containers?topic=containers-ingress-about), assign the user **Administrator** or **Editor** platform access role and the **Manager** service access role for **{{site.data.keyword.cloudcerts_short}}** in **All resource groups**.</li>
  <li>**Viewer** platform access role for the resource group access.</li>
  <li>If your account [restricts service ID creation](/docs/account?topic=account-restrict-service-id-create), the **Service ID creator** role to **Identity and Access Management** in the console (`iam-identity` in the API or CLI).</li>
  <li>If your account [restricts API key creation](/docs/account?topic=account-allow-api-create), the **User API key creator** role to **Identity and Access Management** in the console (`iam-identity` in the API or CLI).</li>
  <li>If you plan to [encrypt your cluster](/docs/containers?topic=containers-encryption#keyprotect):<ul><li>Assign the user the appropriate permission to the key management service (KMS) provider, such as the **Administrator** platform access role for {{site.data.keyword.keymanagementserviceshort}}.</li><li>For clusters that run Kubernetes 1.18.8_1525 or later: When you enable KMS encryption, an additional **Reader** [service-to-service authorization policy](/docs/account?topic=account-serviceauth) between {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.keymanagementserviceshort}} is automatically created for your cluster, if the policy does not already exist. Without this policy, your cluster cannot use all the [{{site.data.keyword.keymanagementserviceshort}} features](/docs/containers?topic=containers-encryption#kms-keyprotect-features).</li></ul></li>
  <li>**Viewer** platform access role for the resource group access.</li></ul>
<li>**Infrastructure**</li><ul>
  <li>Classic clusters only: **Super User** role or the [minimum required permissions](#infra) for classic infrastructure.</li>
  <li>VPC clusters only: **Administrator** platform access role for [**VPC Infrastructure**](/docs/vpc?topic=vpc-iam-getting-started).</li></ul></ul></dd>

<dt>User that creates the cluster</dt>
<dd>In addition to the API key, each individual user must have the following permissions to create a cluster.<ul>
<li>**Administrator** platform access role for **Kubernetes Service** in the console (**containers-kubernetes** in the API or CLI). If your access is scoped to a resource group or region, you must also have the **Viewer** platform access role at the **Account** level to view the account's VLANs.</li>
<li>**Administrator** platform access role for **Container Registry** in the console (**container-registry** in the API or CLI) at the **Account** level.</li>
<li>**Viewer** platform access role to **IAM Identity Service** for account management access.</li>
<li>**Viewer** platform access role for the resource group access.</li></ul></dd>
</dl>

<br>

**More information about assigning permissions**:
* To understand how access works and how to assign users roles in {{site.data.keyword.cloud_notm}} IAM, see [Setting up access to your cluster](/docs/containers?topic=containers-users#access-checklist).
* To create clusters, see [Preparing to create clusters at the account level](/docs/containers?topic=containers-clusters#cluster_prepare).
* For permissions that you might set up for different types of users such as auditors, see [Example use cases and roles](/docs/containers?topic=containers-users#example-iam).

## {{site.data.keyword.cloud_notm}} IAM platform access roles
{: #iam_platform}

{{site.data.keyword.containerlong_notm}} is configured to use {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles. {{site.data.keyword.cloud_notm}} IAM platform access roles determine the actions that users can perform on {{site.data.keyword.cloud_notm}} resources such as clusters, worker nodes, and Ingress application load balancers (ALBs). {{site.data.keyword.cloud_notm}} IAM platform access roles also automatically set basic infrastructure permissions for users. To assign platform access roles, see [Granting users access to your cluster through {{site.data.keyword.cloud_notm}} IAM](/docs/containers?topic=containers-users#platform).
{: shortdesc}

<p class="tip">Do not assign {{site.data.keyword.cloud_notm}} IAM platform access roles at the same time as a service access role. You must assign platform and service access roles separately.</p>

* **Actions requiring no permissions**: Any user in your account who runs the CLI command or makes the API call for the action sees the result, even if the user has no assigned permissions.
* **Viewer actions**: The Viewer platform access role includes the actions that require no permissions, plus the permissions that are shown in the Viewer tab of following table. With the Viewer role, users such as auditors or billing can see cluster details but not modify the infrastructure.
* **Editor actions**: The Editor platform access role includes the permissions that are granted by Viewer, plus the following. With the Editor role, users such as developers can bind services, work with Ingress resources, and set up log forwarding for their apps but cannot modify the infrastructure. Tip: Use this role for app developers, and assign the <a href="#cloud-foundry">Cloud Foundry</a> Developer role.
* **Operator actions**: The Operator platform access role includes the permissions that are granted by Viewer, plus the permissions that are shown in the Operator tab of the following table. With the Operator role, users such as site reliability engineers, DevOps engineers, or cluster administrators can add worker nodes and troubleshoot infrastructure such as by reloading a worker node, but cannot create or delete the cluster, change the credentials, or set up cluster-wide features like service endpoints or managed add-ons.
* **Administrator actions**: The Administrator platform access role includes all permissions that are granted by the Viewer, Editor, and Operator roles, plus the permissions that are show in the Administrator tab of the following table. With the Administrator role, users such as cluster or account administrators can create and delete clusters or set up cluster-wide features like service endpoints or managed add-ons. To create order such infrastructure resources such as worker node machines, VLANs, and subnets, Administrator users need the Super user <a href="#infra">infrastructure role</a> or the API key for the region must be set with the appropriate permissions.

The following table shows the permissions granted by each {{site.data.keyword.cloud_notm}} IAM platform access role. Each tab is organized alphabetically by CLI command name.

| Action | CLI command | API call |
|----|----|----|
| View a list of supported versions for managed add-ons in {{site.data.keyword.containerlong_notm}}. | [`ibmcloud ks addon-versions`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions) | [`GET /v1/addon`](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetAddons) |
| Target or view the API endpoint for {{site.data.keyword.containerlong_notm}}. | [`ibmcloud ks api`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cli_api) | - |
| View a list of supported commands and parameters. | `ibmcloud ks help` | - |
| Initialize the {{site.data.keyword.containerlong_notm}} plug-in or specify the region where you want to create or access Kubernetes clusters. | [`ibmcloud ks init`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init) | - |
| Deprecated: View a list of Kubernetes versions supported in {{site.data.keyword.containerlong_notm}}. | `ibmcloud ks kube-versions`| [`GET /v1/kube-versions`](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetKubeVersions) |
| View a list of available flavors for your worker nodes. | [`ibmcloud ks flavors`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) (machine-types) | [`GET /v2​/getFlavors`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/v2GetFlavors) |
| View current messages for the IBMid user. | [`ibmcloud ks messages`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_messages) | [`GET /v1/messages`](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetMessages) |
| View a list of supported locations in {{site.data.keyword.containerlong_notm}}. | [`ibmcloud ks locations`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) | [`GET /v1/locations`](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/ListLocations) |
| View a list of supported versions in {{site.data.keyword.containerlong_notm}}. | [`ibmcloud ks versions`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_versions_command) | - |
| View a list of available zones that you can create a cluster in. | [`ibmcloud ks zone ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_datacenters) | <ul><li>Classic: [`GET /v1/zones`](https://containers.cloud.ibm.com/global/swagger-global-api/#/util/GetZones)</li><li>VPC: [`GET ​/v2​/vpc​/getZones`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcGetZones)</li></ul> |
{: class="simple-tab-table"}
{: caption="Overview of permissions required for CLI commands and API calls in {{site.data.keyword.containerlong_notm}}." caption-side="top"}
{: #accessreftabtablenone}
{: tab-title="None"}
{: tab-group="access-ref-iam-platform"}

| Action | CLI command | API call |
|----|----|----|
| View information for an Ingress ALB. | [`ibmcloud ks ingress alb get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_get) | <ul><li>Classic: [`GET /v1/albs/{albId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALB)</li><li>VPC: [`GET /v2​/alb​/getAlb`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/V2GetClusterALB)</ul></li>|
| View the Ingress migration status for a cluster. | [`ibmcloud ks ingress alb migrate status`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_migrate_status) | [`GET /v2/alb/getMigrationstatus`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb-beta/getMigrationstatus) |
| List all Ingress ALBs in a cluster. | [`ibmcloud ks ingress alb ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_albs) | <ul><li>Classic: [`GET /v1/clusters/{idOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetClusterALBs)</li><li>VPC: [`GET ​/v2​/alb​/getClusterAlbs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/V2GetClusterALBs)</ul></li>|
| Get the configuration of load balancers that expose Ingress ALBs in your cluster. | [`ibmcloud ks ingress lb get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_lb_proxy-protocol_get) | [`GET /ingress/v2/load-balancer/configuration`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/GetLBConfig) |
| View the name and email address for the owner of the {{site.data.keyword.cloud_notm}} IAM API key for a resource group and region. | [`ibmcloud ks api-key info`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_info) | [`GET /v1/logging/{idOrName}/clusterkeyowner`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetClusterKeyOwner) |
| Download Kubernetes configuration data and certificates to connect to your cluster and run kubectl commands. | [`ibmcloud ks cluster config`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config) | [`GET /v1/clusters/{idOrName}/config`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterConfig) |
| View information for a cluster. | [`ibmcloud ks cluster get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get) | <ul><li>Provider-agnostic: [`GET /v2​/getCluster`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getCluster)</li><li>Classic: [`GET /v1/clusters/{idOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetCluster)</li><li>VPC: [`GET /v2​/vpc/getCluster`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcGetCluster)</li></ul> |
| List all services in all namespaces that are bound to a cluster. | [`ibmcloud ks cluster service ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_services) | [`GET /v1/clusters/{idOrName}/services`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesForAllNamespaces) |
| List all clusters. | [`ibmcloud ks cluster ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_clusters) | <ul><li>Classic: [`GET /v1/clusters`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusters)</li><li>VPC: [`GET ​/v2​/vpc​/getClusters`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcGetClusters)</li></ul> |
| Get the infrastructure credentials that are set for the {{site.data.keyword.cloud_notm}} account to access a different classic infrastructure portfolio. | [`ibmcloud ks credential get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get) | [`GET /v1/credentials`](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetUserCredentials) |
| Check whether the credentials that allow access to the classic IBM Cloud infrastructure portfolio for the targeted region and resource group are missing suggested or required infrastructure permissions. | [`ibmcloud ks infra-permissions get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) | [`GET /v1/infra-permissions`](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetInfraPermissions) |
| View the status for automatic updates of the Fluentd add-on. | [`ibmcloud ks logging autoupdate get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_get) | [`GET /v1/logging/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetUpdatePolicy) |
| View the default logging endpoint for the targeted region. | - | [`GET /v1/logging/{idOrName}/default`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/GetDefaultLoggingEndpoint) |
| List all log forwarding configurations in the cluster or for a specific log source in the cluster. | [`ibmcloud ks logging config get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_get) | [`GET /v1/logging/{idOrName}/loggingconfig`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigs) and [`GET /v1/logging/{idOrName}/loggingconfig/{logSource}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/FetchLoggingConfigsForSource) |
| View information for a log filtering configuration. | [`ibmcloud ks logging filter get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view) | [`GET /v1/logging/{idOrName}/filterconfigs/{id}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfig) |
| List all logging filter configurations in the cluster. | [`ibmcloud ks logging filter get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_view) | [`GET /v1/logging/{idOrName}/filterconfigs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/FetchFilterConfigs) |
| List all services that are bound to a specific namespace. | - | [`GET /v1/clusters/{idOrName}/services/{namespace}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ListServicesInNamespace) |
| List all IBM Cloud infrastructure subnets that are bound to a cluster. | - | [`GET /v1/clusters/{idOrName}/subnets`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterSubnets) |
| List all user-managed subnets that are bound to a cluster. | - | [`GET /v1/clusters/{idOrName}/usersubnets`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterUserSubnet) |
| List available subnets in all resource groups. | [`ibmcloud ks subnets`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_subnets) | <ul><li>Classic: [`GET /v1/subnets`](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/ListSubnets)</li><li>VPC: [`GET /v2/vpc/getSubnets`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getSubnets)</li></ul> |
| View the VLAN spanning status for the infrastructure account. | [`ibmcloud ks vlan spanning get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) | [`GET /v1/subnets/vlan-spanning`](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/GetVlanSpanning) |
| When set for one cluster: List VLANs that the cluster is connected to in a zone.</br>When set for all clusters in the account: List all available VLANs in a zone. | [`ibmcloud ks vlan ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans) | [`GET /v1/datacenters/{datacenter}/vlans`](https://containers.cloud.ibm.com/global/swagger-global-api/#/properties/GetDatacenterVLANs) |
| List all VPCs in the targeted resource group. | [`ibmcloud ks vpcs`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vpcs) | [`GET /v2​/vpc​/getVPCs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getVPCs) |
| List all webhooks for a cluster. | - | [`GET /v1/clusters/{idOrName}/webhooks`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWebhooks) |
| View information for a worker node. | [`ibmcloud ks worker get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_get) | <ul><li>Provider-agnostic: [`GET /v2/getWorker`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getWorker)</li><li>Classic: [`GET /v2/classic/getWorker`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/classicGetWorker)</li><li>VPC: [`GET /v2​/vpc​/getWorker`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcGetWorker)</li></ul> |
| View information for a worker pool. | [`ibmcloud ks worker-pool get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_get) | <ul><li>Classic: [`GET /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPool)</li><li>VPC: [`GET /v2/getWorkerPool`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getWorkerPool)</li></ul> |
| List all worker pools in a cluster. | [`ibmcloud ks worker-pool ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pools) | <ul><li>Classic: [`GET /v1/clusters/{idOrName}/workerpools`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetWorkerPools)</li><li>VPC: [`GET /v2/getWorkerPools`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getWorkerPools)</li></ul> |
| List all worker nodes in a cluster. | [`ibmcloud ks worker ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_workers) | <ul><li>Provider-agnostic: [`GET/v2/getWorkers`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/getWorkers)</li><li>Classic: [`GET /v2/classic/getWorkers`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/classicGetWorkers)</li><li>VPC: [`GET /v2/vpc/getWorkers`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcGetWorkers)</li></ul> |
{: class="simple-tab-table"}
{: caption="Overview of permissions required for CLI commands and API calls in {{site.data.keyword.containerlong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the action that you can take with {{site.data.keyword.containerlong_notm}} service. The second column is the name of the action in the command line interface (CLI). The third column is the name of the action in the application programming interface (API)."}
{: #accessreftabtableview}
{: tab-title="Viewer"}
{: tab-group="access-ref-iam-platform"}

| Action | CLI command | API call |
|----|----|----|
| Disable automatic updates for the Ingress ALB add-on. | [`ibmcloud ks ingress alb autoupdate disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable) | [`PUT /v1/clusters/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy) |
| Enable automatic updates for the Ingress ALB add-on. | [`ibmcloud ks ingress alb autoupdate enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable) | [`PUT /v1/clusters/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/ChangeUpdatePolicy) |
| Check whether automatic updates for the Ingress ALB add-on are enabled. | [`ibmcloud ks ingress alb autoupdate get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get) | [`GET /v1/clusters/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/GetUpdatePolicy) |
| Enable or disable an Ingress ALB in a classic cluster. | [`ibmcloud ks ingress alb configure classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure) | [`POST /v1/albs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/EnableALB) and [`DELETE /v1/albs/{albId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/) |
| Create an Ingress ALB in a classic cluster. | [`ibmcloud ks ingress alb create classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create) | [`POST /v1/clusters/{idOrName}/zone/{zoneId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/CreateALB) |
| Force a one-time update of your Ingress ALB pods. | [`ibmcloud ks ingress alb update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update) | [`PUT /v1/alb/versions`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs) |
| View the available images and image versions for Ingress ALBs in your cluster. | [`ibmcloud ks ingress alb versions`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_versions) | [`PUT /v1/alb/clusters/{idOrName}/update`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb/UpdateALBs) |
| Create an API server audit webhook. | [`ibmcloud ks cluster master audit-webhook set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_set) | [`PUT /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/apiserverconfigs/UpdateAuditWebhook) |
| Delete an API server audit webhook. | [`ibmcloud ks cluster master audit-webhook unset`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_config_unset) | [`DELETE /v1/clusters/{idOrName}/apiserverconfigs/auditwebhook`](https://containers.cloud.ibm.com/global/swagger-global-api/#/apiserverconfigs/DeleteAuditWebhook) |
| Bind a service to a cluster. **Note**: You must have the Cloud Foundry Developer role for the space that you service instance is in. | [`ibmcloud ks cluster service bind`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) | [`POST /v1/clusters/{idOrName}/services`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/BindServiceToNamespace) |
| Unbind a service from a cluster. **Note**: You must have the Cloud Foundry Developer role for the space that you service instance is in. | [`ibmcloud ks cluster service unbind`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_unbind) | [`DELETE /v1/clusters/{idOrName}/services/{namespace}/{serviceInstanceId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UnbindServiceFromNamespace) |
| Create a log forwarding configuration. | [`ibmcloud ks logging config create`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_create) | [`POST /v1/logging/{idOrName}/loggingconfig/{logSource}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/CreateLoggingConfig) |
| Refresh a log forwarding configuration. | [`ibmcloud ks logging refresh`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_refresh) | [`PUT /v1/logging/{idOrName}/refresh`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/RefreshLoggingConfig) |
| Delete a log forwarding configuration. | [`ibmcloud ks logging config rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_rm) | [`DELETE /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfig) |
| Delete all log forwarding configurations for a cluster. | - | [`DELETE /v1/logging/{idOrName}/loggingconfig`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/DeleteLoggingConfigs) |
| Update a log forwarding configuration. | [`ibmcloud ks logging config update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_logging_update) | [`PUT /v1/logging/{idOrName}/loggingconfig/{logSource}/{id}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/UpdateLoggingConfig) |
| Create a log filtering configuration. | [`ibmcloud ks logging filter create`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_create) | [`POST /v1/logging/{idOrName}/filterconfigs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/CreateFilterConfig) |
| Delete a log filtering configuration. | [`ibmcloud ks logging filter rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_delete) | [`DELETE /v1/logging/{idOrName}/filterconfigs/{id}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfig) |
| Delete all logging filter configurations for the Kubernetes cluster. | - | [`DELETE /v1/logging/{idOrName}/filterconfigs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/DeleteFilterConfigs) |
| Update a log filtering configuration. | [`ibmcloud ks logging filter update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_filter_update) | [`PUT /v1/logging/{idOrName}/filterconfigs/{id}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/filter/UpdateFilterConfig) |
| Configure and optionally enable a health check monitor for an existing NLB subdomain in a cluster. | [`ibmcloud ks nlb-dns monitor configure`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure) | [`POST /v1/health/clusters/{idOrName}/config`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/AddNlbDNSHealthMonitor) |
| View the settings for an existing health check monitor. | [`ibmcloud ks nlb-dns monitor get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-get) | [`GET /v1/health/clusters/{idOrName}/host/{nlbHost}/config`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/GetNlbDNSHealthMonitor) |
| Disable an existing health check monitor for a subdomain in a cluster. | [`ibmcloud ks nlb-dns monitor disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-disable) | [`PUT /v1/clusters/{idOrName}/health`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/UpdateNlbDNSHealthMonitor) |
| Enable a health check monitor that you configured. | [`ibmcloud ks nlb-dns monitor enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-enable) | [`PUT /v1/clusters/{idOrName}/health`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/UpdateNlbDNSHealthMonitor) |
| List the health check monitor settings for each NLB subdomain in a cluster. | [`ibmcloud ks nlb-dns monitor ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-ls) | [`GET /v1/health/clusters/{idOrName}/list`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/ListNlbDNSHealthMonitors) |
| List the health check status of each IP address that is registered with an NLB subdomain in a cluster. | [`ibmcloud ks nlb-dns monitor status`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-status) | [`GET /v1/health/clusters/{idOrName}/status`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-health-monitor/ListNlbDNSHealthMonitorStatus) |
| Add one NLB IP address to an existing NLB subdomain. | [`ibmcloud ks nlb-dns add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-add) | [`PUT /v1/clusters/{idOrName}/add`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-dns/UpdateDNSWithIP) |
| Create a DNS subdomain to register an NLB IP address. | [`ibmcloud ks nlb-dns create classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-create) | [`POST /v1/clusters/{idOrName}/register`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-dns/RegisterDNSWithIP) |
| List NLB subdomains and either the NLB IP addresses (classic clusters) or the load balancer hostnames (VPC clusters) that are registered with the DNS provider for each NLB subdomain. | [`ibmcloud ks nlb-dns ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-ls) | <ul><li>Classic: [`GET /v1/clusters/{idOrName}/list`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-dns/ListNLBIPsForSubdomain)</li><li>VPC: [`GET /v2/nlb-dns/getNlbDNSList`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/GetNlbDNSList)</li></ul> |
| Replace the VPC load balancer hostname for a subdomain. | [`ibmcloud ks nlb-dns replace`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-replace) | [`POST /v2/nlb-dns/vpc/replaceLBHostname`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/ReplaceLBHostname) |
| Remove an NLB IP address from a subdomain. | [`ibmcloud ks nlb-dns rm classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-rm) | [`DELETE /v1/clusters/{idOrName}/host/{nlbHost}/ip/{nlbIP}/remove`](https://containers.cloud.ibm.com/global/swagger-global-api/#/nlb-dns/UnregisterDNSWithIP) |
| Regenerate the certificate and secret for an NLB subdomain. | [`ibmcloud ks nlb-dns secret regenerate`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-secret-regenerate) | [`POST ​/v2​/nlb-dns​/regenerateCert`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/RegenerateCert) |
| Delete a secret from an NLB subdomain and prevent future renewal of the certificate. | [`ibmcloud ks nlb-dns secret rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-secret-rm) | [`POST ​/v2​/nlb-dns​/deleteSecret`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/DeleteSecret) |
| Create a webhook in a cluster. | [`ibmcloud ks webhook-create`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_webhook_create) | [`POST /v1/clusters/{idOrName}/webhooks`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWebhooks) |
{: class="simple-tab-table"}
{: caption="Overview of permissions required for CLI commands and API calls in {{site.data.keyword.containerlong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the action that you can take with {{site.data.keyword.containerlong_notm}} service. The second column is the name of the action in the command line interface (CLI). The third column is the name of the action in the application programming interface (API)."}
{: #accessreftabtableedit}
{: tab-title="Editor"}
{: tab-group="access-ref-iam-platform"}

| Action | CLI command | API call |
|----|----|----|
| Refresh the Kubernetes master. | [`ibmcloud ks cluster master refresh`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) | [`PUT /v1/clusters/{idOrName}/masters`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/HandleMasterAPIServer) |
| Update a cluster. | [`ibmcloud ks cluster master update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update) | [`PUT /v1/clusters/{idOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateCluster) |
| Make an {{site.data.keyword.cloud_notm}} IAM service ID for the cluster, create a policy for the service ID that assigns the **Reader** service access role in {{site.data.keyword.registrylong_notm}}, and then create an API key for the service ID. | [`ibmcloud ks cluster pull-secret apply`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply) | - |
| Add a subnet to a cluster. | [`ibmcloud ks cluster subnet add`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) | [`PUT /v1/clusters/{idOrName}/subnets/{subnetId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterSubnet) |
| Create a subnet and add it to a cluster. | [`ibmcloud ks cluster subnet create`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_create) | [`POST /v1/clusters/{idOrName}/vlans/{vlanId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateClusterSubnet) |
| Detach a subnet from a cluster. | [`ibmcloud ks cluster subnet detach`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_detach) | [`DELETE /v1/clusters/{idOrName}/subnets/{subnetId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/DetachClusterSubnet) |
| Update the configuration of load balancers that expose Ingress ALBs in your cluster. | [`ibmcloud ks ingress lb proxy-protocol disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_lb_proxy-protocol_disable) and [`ibmcloud ks ingress lb proxy-protocol enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_lb_proxy-protocol_enable) | [`PATCH /ingress/v2/load-balancer/configuration`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/PatchLBConfig) |
| Add worker nodes. | [`ibmcloud ks worker add (deprecated)`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_add) | [`POST /v1/clusters/{idOrName}/workers`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddClusterWorkers) |
| Create a worker pool in a classic cluster. | [`ibmcloud ks worker-pool create classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create) | [`POST /v1/clusters/{idOrName}/workerpools`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateWorkerPool) |
| Rebalance a worker pool. | [`ibmcloud ks worker-pool rebalance`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_rebalance) | [`PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) |
| Resize a worker pool. | [`ibmcloud ks worker-pool resize`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_resize) | [`PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool) |
| Set labels on a worker pool. | [`ibmcloud ks worker-pool label set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_label_set) | <ul><li>**v1 API**: </li>[`PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)<li>**v2 API**: [`POST /v2/setWorkerPoolLabels`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/v2SetWorkerPoolLabels)</li></ul>|
| Remove labels from a worker pool. | [`ibmcloud ks worker-pool label rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_label_rm) | <ul><li>**v1 API**: </li>[`PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/PatchWorkerPool)<li>**v2 API**: [`POST /v2/setWorkerPoolLabels`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/v2SetWorkerPoolLabels)</li></ul>|
| Delete a worker pool. | [`ibmcloud ks worker-pool rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_rm) | [`DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPool) |
| Reboot a worker node. | [`ibmcloud ks worker reboot`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) | [`PUT /v1/clusters/{idOrName}/workers/{workerId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker) |
| Reload a worker node. | [`ibmcloud ks worker reload`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) | [`PUT /v1/clusters/{idOrName}/workers/{workerId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker) |
| Replace a worker node. | [`ibmcloud ks worker replace`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cli_worker_replace) | <ul><li>Classic: [`POST /v2​/replaceWorker`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/replaceWorker)</li><li>VPC: [`POST /v2​/vpc​/replaceWorker`](https://containers.cloud.ibm.com/global/swagger-global-api/#/v2/vpcReplaceWorker)</li></ul> |
| Remove a worker node. | [`ibmcloud ks worker rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_rm) | [`DELETE /v1/clusters/{idOrName}/workers/{workerId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveClusterWorker) |
| Update a worker node. | [`ibmcloud ks worker update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) | [`PUT /v1/clusters/{idOrName}/workers/{workerId}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/UpdateClusterWorker) |
| Add a zone to a worker pool. | [`ibmcloud ks zone add classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_add) | [`POST /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZone) |
| Update the network configuration for a given zone in a worker pool. | [`ibmcloud ks zone network-set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_network_set) | [`PATCH /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/AddWorkerPoolZoneNetwork) |
| Remove a zone a from worker pool. | [`ibmcloud ks zone rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_zone_rm) | [`DELETE /v1/clusters/{idOrName}/workerpools/{poolidOrName}/zones/{zoneid}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveWorkerPoolZone) |
{: class="simple-tab-table"}
{: caption="Overview of permissions required for CLI commands and API calls in {{site.data.keyword.containerlong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the action that you can take with {{site.data.keyword.containerlong_notm}} service. The second column is the name of the action in the command line interface (CLI). The third column is the name of the action in the application programming interface (API)."}
{: #accessreftabtableoper}
{: tab-title="Operator"}
{: tab-group="access-ref-iam-platform"}

| Action | CLI command | API call |
|----|----|----|
| Create a secret for a certificate from your {{site.data.keyword.cloudcerts_long_notm}} instance to your cluster. | [`ibmcloud ks ingress secret create`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_create) | [`POST ​/ingress​/v2​/secret​/createSecret`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/CreateSecret) |
| View details for an Ingress secret in a cluster. | [`ibmcloud ks ingress secret get`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_get) | [`GET ​/ingress​/v2​/secret​/getSecret`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/GetSecret) |
| List all Ingress secrets in a cluster. | [`ibmcloud ks ingress secret ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_ls) | [`GET ​/ingress​/v2​/secret​/getSecrets`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/GetSecrets) |
| Remove an Ingress secret from a cluster. | [`ibmcloud ks ingress secret rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_rm) | [`POST ​/ingress​/v2​/secret​/deleteSecret`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/DeleteIngressSecret) |
| Update an Ingress secret in your cluster. | [`ibmcloud ks ingress secret update`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_update) | [`POST ​/ingress​/v2​/secret​/updateSecret`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/UpdateSecret) |
| Clean up Ingress resources and configmaps, such as after an Ingress migration. | [`ibmcloud ks ingress alb migrate clean`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_migrate_clean) | [`POST /v2/alb/cleanupMigration`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb-beta/cleanupMigration) |
| Start a migration of Ingress resources and configmaps. | [`ibmcloud ks ingress alb migrate start`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_migrate_start) | [`POST /v2/alb/startMigration`](https://containers.cloud.ibm.com/global/swagger-global-api/#/alb-beta/startMigration) |
| Set the API key for the {{site.data.keyword.cloud_notm}} account to access the linked IBM Cloud infrastructure portfolio. | [`ibmcloud ks api-key reset`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) | [`POST /v1/keys`](https://containers.cloud.ibm.com/global/swagger-global-api/#/accounts/ResetUserAPIKey) |
| Disable a managed add-on, such the Kubernetes web terminal, in a cluster. | [`ibmcloud ks cluster addon disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) | [`PATCH /v1/clusters/{idOrName}/addons`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons) |
| Enable a managed add-on, such the Kubernetes web terminal, in a cluster. | [`ibmcloud ks cluster addon enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) | [`PATCH /v1/clusters/{idOrName}/addons`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/ManageClusterAddons) |
| List managed add-ons, such as the Kubernetes web terminal, that are enabled in a cluster. | [`ibmcloud ks cluster addon ls`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons) | [`GET /v1/clusters/{idOrName}/addons`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterAddons) |
| Create a free or standard cluster on classic infrastructure. **Note**: The Administrator platform access role for {{site.data.keyword.registrylong_notm}} and the Super User infrastructure role are also required. | [`ibmcloud ks cluster create classic`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) | [`POST /v1/clusters`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/CreateCluster) |
| Enable the private cloud service endpoint for the cluster master. | [`ibmcloud ks cluster master private-service-endpoint enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_master_pse_enable) | [`POST ​/v2​/enablePrivateServiceEndpoint`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/v2EnablePrivateServiceEndpoint) |
| Disable the public cloud service endpoint for the cluster master. | [`ibmcloud ks master public-service-endpoint disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_master_pub_se_disable) | [`POST ​/v2​/disablePublicServiceEndpoint`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/v2DisablePublicServiceEndpoint) |
| Enable the public cloud service endpoint for the cluster master. | [`ibmcloud ks master public-service-endpoint enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_master_pub_se_enable) | [`POST ​/v2​/enablePublicServiceEndpoint`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/v2EnablePublicServiceEndpoint) |
| Delete a cluster. | [`ibmcloud ks cluster rm`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm) | [`DELETE /v1/clusters/{idOrName}`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster) |
| Set infrastructure credentials for the {{site.data.keyword.cloud_notm}} account to access a different classic infrastructure portfolio. | [`ibmcloud ks credential set`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) | [`POST /v1/credentials`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/StoreUserCredentials) |
| Remove infrastructure credentials for the {{site.data.keyword.cloud_notm}} account to access a different classic infrastructure portfolio. | [`ibmcloud ks credential unset`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset) | [`DELETE /v1/credentials`](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/accounts/RemoveUserCredentials) |
| Encrypt Kubernetes secrets by using a key management service (KMS) provider. | [`ibmcloud ks kms enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#ks_kms_enable) | [`POST /v2/enableKMS`](https://containers.cloud.ibm.com/global/swagger-global-api/#/beta/kmsEnableCluster) |
| Disable automatic updates for the Fluentd cluster add-on. | [`ibmcloud ks logging autoupdate disable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_disable) | [`PUT /v1/logging/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy) |
| Enable automatic updates for the Fluentd cluster add-on. | [`ibmcloud ks logging autoupdate enable`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_autoupdate_enable) | [`PUT /v1/logging/{idOrName}/updatepolicy`](https://containers.cloud.ibm.com/global/swagger-global-api/#/logging/ChangeUpdatePolicy) |
| Collect a snapshot of API server logs in an {{site.data.keyword.cos_full_notm}} bucket. | [`ibmcloud ks logging collect`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) | [`POST /v1/log-collector/{idOrName}/masterlogs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/CreateMasterLogCollection) |
| See the status of the API server logs snapshot request. | [`ibmcloud ks logging collect-status`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) | [`GET /v1/log-collector/{idOrName}/masterlogs`](https://containers.cloud.ibm.com/global/swagger-global-api/#/log45collector/GetMasterLogCollectionStatus) |
{: class="simple-tab-table"}
{: caption="Overview of permissions required for CLI commands and API calls in {{site.data.keyword.containerlong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the action that you can take with {{site.data.keyword.containerlong_notm}} service. The second column is the name of the action in the command line interface (CLI). The third column is the name of the action in the application programming interface (API)."}
{: #accessreftabtableadmin}
{: tab-title="Administrator"}
{: tab-group="access-ref-iam-platform"}

<br />

## {{site.data.keyword.cloud_notm}} IAM service access roles
{: #service}

Every user who is assigned an {{site.data.keyword.cloud_notm}} IAM service access role is also automatically assigned a corresponding Kubernetes role-based access control (RBAC) role in a specific namespace. To assign service access roles, see [Granting users access to your cluster through {{site.data.keyword.cloud_notm}} IAM](/docs/containers?topic=containers-users#platform). Do not assign {{site.data.keyword.cloud_notm}} IAM platform access roles at the same time as a service access role. You must assign platform and service access roles separately.
{: shortdesc}

Looking for which Kubernetes actions each service access role grants through RBAC? See [Kubernetes resource permissions per RBAC role](#rbac_ref). To learn more about RBAC roles, see [Assigning RBAC permissions](/docs/containers?topic=containers-users#role-binding) and [Extending existing permissions by aggregating cluster roles](/docs/containers?topic=containers-users#rbac_aggregate). For the username details, see [{{site.data.keyword.cloud_notm}} IAM issuer details for RBAC users](#iam_issuer_users).
{: tip}

The following table shows the Kubernetes resource permissions that are granted by each service access role and its corresponding RBAC role.

<table summary="The columns are read from left to right. The first column has the service access role. The second column has the RBAC role, binding, and scope for the service access role. The third column describes the permissions that the service access role gives.">
<caption>Kubernetes resource permissions by service and corresponding RBAC roles</caption>
<col width="25%">
<thead>
    <th id="service-role">service access role</th>
    <th id="rbac-role">Corresponding RBAC role, binding, and scope</th>
    <th id="kube-perm">Kubernetes resource permissions</th>
</thead>
<tbody>
  <tr>
    <td id="service-role-reader" headers="service-role">Reader role</td>
    <td headers="service-role-reader rbac-role">When scoped to one namespace: <strong><code>view</code></strong> cluster role applied by the <strong><code>ibm-view</code></strong> role binding in that namespace</br><br>When scoped to all namespaces: <strong><code>view</code></strong> cluster role applied by the <strong><code>ibm-view</code></strong> role binding in each namespace of the cluster. You can also view the cluster in the {{site.data.keyword.cloud_notm}} console and CLI.</td>
    <td headers="service-role-reader kube-perm"><ul>
      <li>Read access to resources in a namespace</li>
      <li>No read access to roles and role bindings or to Kubernetes secrets</li>
      <li>Access the Kubernetes dashboard to view resources in a namespace</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-writer" headers="service-role">Writer role</td>
    <td headers="service-role-writer rbac-role">When scoped to one namespace: <strong><code>edit</code></strong> cluster role applied by the <strong><code>ibm-edit</code></strong> role binding in that namespace</br><br>When scoped to all namespaces: <strong><code>edit</code></strong> cluster role applied by the <strong><code>ibm-edit</code></strong> role binding in each namespace of the cluster</td>
    <td headers="service-role-writer kube-perm"><ul><li>Read/write access to resources in a namespace</li>
    <li>No read/write access to roles and role bindings</li>
    <li>Access the Kubernetes dashboard to view resources in a namespace</li></ul>
    </td>
  </tr>
  <tr>
    <td id="service-role-manager" headers="service-role">Manager role</td>
    <td headers="service-role-manager rbac-role">When scoped to one namespace: <strong><code>admin</code></strong> cluster role applied by the <strong><code>ibm-operate</code></strong> role binding in that namespace</br><br>When scoped to all namespaces: <strong><code>cluster-admin</code></strong> cluster role applied by the <strong><code>ibm-admin</code></strong> cluster role binding that applies to all namespaces</td>
    <td headers="service-role-manager kube-perm">When scoped to one namespace:
      <ul><li>Read/write access to all resources in a namespace but not to resource quota or the namespace itself</li>
      <li>Create RBAC roles and role bindings in a namespace</li>
      <li>Access the Kubernetes dashboard to view all resources in a namespace</li></ul>
    </br>When scoped to all namespaces:
        <ul><li>Read/write access to all resources in every namespace</li>
        <li>Create RBAC roles and role bindings in a namespace or cluster roles and cluster role bindings in all namespaces</li>
        <li>Access the Kubernetes dashboard</li>
        <li>Create an Ingress resource that makes apps publicly available</li>
        <li>Review cluster metrics such as with the <code>kubectl top pods</code>, <code>kubectl top nodes</code>, or <code>kubectl get nodes</code> commands</li>
        <li>[Create and update privileged and unprivileged (restricted) pods](/docs/containers?topic=containers-psp#customize_psp)</li></ul>
    </td>
  </tr>
  </tr>
</tbody>
</table>

<br />

## Kubernetes resource permissions per RBAC role
{: #rbac_ref}

Every user who is assigned an {{site.data.keyword.cloud_notm}} IAM service access role is also automatically assigned a corresponding, predefined Kubernetes role-based access control (RBAC) role. If you plan to manage your own custom Kubernetes RBAC roles, see [Creating custom RBAC permissions for users, groups, or service accounts](/docs/containers?topic=containers-users#rbac). For the username details, see [{{site.data.keyword.cloud_notm}} IAM issuer details for RBAC users](#iam_issuer_users).
{: tip}
{: shortdesc}

Wondering if you have the correct permissions to run a certain `kubectl` command on a resource in a namespace? Try the [`kubectl auth can-i` command](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-can-i-em-){: external}.
{: tip}

The following table shows the permissions that are granted by each RBAC role to individual Kubernetes resources. Permissions are shown as which verbs a user with that role can complete against the resource, such as "get", "list", "describe", "create", or "delete".

<table summary="The columns are read from left to right. The first column has the Kubernetes resource that an RBAC role authorizes actions to. The second column describes the actions that the view role authorizes for the resource. The third column describes the actions that the edit role authorizes for the resource. The fourth column describes the actions that the admin and cluster admin roles authorize for the resource.">
 <caption>Kubernetes resource permissions granted by each predefined RBAC role</caption>
 <col width="25%">
 <thead>
  <th>Kubernetes resource</th>
  <th><code>view</code></th>
  <th><code>edit</code></th>
  <th><code>admin</code> and <code>cluster-admin</code></th>
 </thead>
<tbody>
<tr>
  <td><code>bindings</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**cluster-admin only:** <code>create</code>, <code>delete</code>, <code>update</code></td>
</tr><tr>
  <td><code>configmaps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>cronjobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.apps </code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>daemonsets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/rollback</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>deployments.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>endpoints</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>events</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>horizontalpodautoscalers.autoscaling</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>ingresses.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>jobs.batch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>limitranges</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>localsubjectaccessreviews</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code></td>
</tr><tr>
  <td><code>namespaces</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></br>**cluster-admin only:** <code>create</code>, <code>delete</code></td>
</tr><tr>
  <td><code>namespaces/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>networkpolicies.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>node</code></td>
  <td>None</td>
  <td>None</td>
  <td>`admin` scoped to a namespace: None<br><br>
  `cluster-admin` for all namespaces: All verbs</td>
</tr><tr>
  <td><code>persistentvolume</code></td>
  <td>None</td>
  <td>None</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>persistentvolumeclaims</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>poddisruptionbudgets.policy</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>top</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/attach</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/exec</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/log</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/portforward</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>pods/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicasets.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>replicationcontrollers.extensions/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>resourcequotas/status</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
</tr><tr>
  <td><code>rolebindings</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>roles</code></td>
  <td>-</td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>secrets</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>serviceaccounts</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code>, <code>impersonate</code></td>
</tr><tr>
  <td><code>services</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>services/proxy</code></td>
  <td>-</td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr><tr>
  <td><code>statefulsets.apps/scale</code></td>
  <td><code>get</code>, <code>list</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
  <td><code>create</code>, <code>delete</code>, <code>deletecollection</code>, <code>get</code>, <code>list</code>, <code>patch</code>, <code>update</code>, <code>watch</code></td>
</tr>
</tbody>
</table>

<br />

## {{site.data.keyword.cloud_notm}} IAM issuer details for RBAC users
{: #iam_issuer_users}

Users with a service access role to {{site.data.keyword.containerlong_notm}} in IAM are given [corresponding user roles in RBAC](#rbac_ref). The RBAC user details include a unique issuer ID, subject identifier claim, and Kubernetes username. These details vary with the Kubernetes version of the cluster. When you update a cluster from a previous version, the details are automatically updated. RBAC usernames are prefixed by `IAM#`. For more information about how OpenID authentication works, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authentication/){: external}.
{: shortdesc}

You might use this information if you build automation tooling within the cluster that relies on the user details to authenticate with the Kubernetes API server.

| Version | Issuer | Claim | Casing`*` |
| --- | --- | --- | --- |
| 1.18 or later | `https://iam.cloud.ibm.com/identity` | `realmed_sub_<account_ID>` | lowercase |
| 1.17 | `https://iam.cloud.ibm.com/identity` | `sub_<account_ID>` | lowercase |
| 1.10 - 1.16 | `https://iam.bluemix.net/identity` | `sub_<account_ID>` | lowercase |
| 1.9 or earlier | `https://iam.ng.bluemix.net/kubernetes` | `sub` | camel case |
{: summary="The rows are read from left to right. The first column is the Kubernetes version of the cluster. The second column is the {{site.data.keyword.cloud_notm}} IAM Issuer ID. The third column is the subject identifier claim. The fourth column is the casing style of the username."}
{: caption="{{site.data.keyword.cloud_notm}} IAM issuer details for RBAC users" caption-side="top"}

`*`: An example of lowercase is `user.name@company.com`. An example of camel case is `User.Name@company.com`.
{: note}

<br />

## Cloud Foundry roles
{: #cloud-foundry}

Cloud Foundry roles grant access to organizations and spaces within the account. To see the list of Cloud Foundry-based services in {{site.data.keyword.cloud_notm}}, run `ibmcloud service list`. To learn more, see all available [org and space roles](/docs/account?topic=account-cfaccess) or the steps for [managing Cloud Foundry access](/docs/account?topic=account-mngcf) in the {{site.data.keyword.cloud_notm}} IAM documentation.
{: shortdesc}

The following table shows the Cloud Foundry roles that are required for cluster action permissions.

<table summary="The columns are read from left to right. The first column has the Cloud Foundry role. The second column describes permissions for the role.">
  <caption>Cluster management permissions by Cloud Foundry role</caption>
  <col width="25%">
  <thead>
    <th>Cloud Foundry role</th>
    <th>Cluster management permissions</th>
  </thead>
  <tbody>
  <tr>
    <td>Space role: Manager</td>
    <td>Manage user access to an {{site.data.keyword.cloud_notm}} space</td>
  </tr>
  <tr>
    <td>Space role: Developer</td>
    <td>
      <ul><li>Create {{site.data.keyword.cloud_notm}} service instances</li>
      <li>Bind {{site.data.keyword.cloud_notm}} service instances to clusters</li>
      <li>View logs from a cluster's log forwarding configuration at the space level</li></ul>
    </td>
  </tr>
  </tbody>
</table>

## Classic infrastructure roles
{: #infra}

A user with the **Super User** infrastructure access role [sets the API key for a region and resource group](/docs/containers?topic=containers-users#api_key) so that infrastructure actions can be performed (or more rarely, [manually sets different account credentials](/docs/containers?topic=containers-users#credentials)). Then, the infrastructure actions that other users in the account can perform is authorized through {{site.data.keyword.cloud_notm}} IAM platform access roles. You do not need to edit the other users' classic infrastructure permissions. Use the following table to customize users' classic infrastructure permissions only when you can't assign **Super User** to the user who sets the API key. For instructions to assign permissions, see [Customizing infrastructure permissions](/docs/containers?topic=containers-users#infra_access).
{: shortdesc}


<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic infrastructure permissions apply only to classic clusters. For VPC clusters, see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).
{: note}


Need to check that the API key or manually-set credentials have the required and suggested infrastructure permissions? Use the `ibmcloud ks infra-permissions get` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get).
{: tip}

The following table shows the classic infrastructure permissions that the credentials for a region and resource group can have for creating clusters and other common use cases. The description includes how you can assign the permission in the {{site.data.keyword.cloud_notm}} IAM Classic infrastructure console or the `ibmcloud sl` command. For more information, see the instructions for the [console](/docs/containers?topic=containers-users#infra_console) or [CLI](/docs/containers?topic=containers-users#infra_cli).
*   **Create clusters**: Classic infrastructure permissions that you must have to create a cluster. When you run `ibmcloud ks infra-permissions get`, these permissions are listed as **Required**. For other service permissions that you must have in {{site.data.keyword.cloud_notm}} IAM to create clusters, see [Permissions to create a cluster](#cluster_create_permissions).
*   **Other common use cases**: Classic infrastructure permissions that you must have for other common scenarios. Even if you have permission to create a cluster, some limitations might apply. For example, you might not be able to create or work with a cluster with bare metal worker nodes or a public IP address. After cluster creation, further steps to add networking or storage resources might fail. When you run `ibmcloud ks infra-permissions get`, these permissions are listed as **Suggested**.

| Permission | Description | IAM Assign Policy Console | CLI |
|:-----------------|:-----------------|:---------------|:----|
| IPMI Remote Management | Manage worker nodes.|Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission REMOTE_MANAGEMENT --enable true</code></pre> |
| Add Server | Add worker nodes. <br><br> **Note**: For worker nodes that have public IP addresses, you also need the **Add Compute with Public Network Port** permission in the **Network** category. | Add Server: Classic infrastructure > Permissions > Account<br><br> Add Compute with Public Network Port: Classic infrastructure > Permissions > Network |Add Server: <pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission SERVER_ADD --enable true</code></pre>  Add Compute with Public Network Port: <pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission PUBLIC_NETWORK_COMPUTE --enable true</code></pre>|
| Cancel Server | Delete worker nodes. | Classic infrastructure > Permissions > Account|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission SERVER_CANCEL --enable true</code></pre>  |
| OS Reloads and Rescue Kernel | Update, reboot, and reload worker nodes. | Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission SERVER_RELOAD --enable true</code></pre>  |
| View Virtual Server Details | Required if the cluster has VM worker nodes. List and get details of VM worker nodes. | Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission VIRTUAL_GUEST_VIEW --enable true</code></pre>  |
| View Hardware Details | Required if the cluster has bare metal worker nodes. List and get details of bare metal worker nodes. | Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission HARDWARE_VIEW --enable true</code></pre>  |
| Add Support Case | As part of the cluster creation automation, support cases are opened to provision the cluster infrastructure. | - |<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission TICKET_ADD --enable true</code></pre>  |
| Edit Support Case | As part of the cluster creation automation, support cases are updated to provision the cluster infrastructure. | - |<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission TICKET_EDIT --enable true</code></pre>  |
| View Support Case | As part of the cluster creation automation, support cases are used to provision the cluster infrastructure. | - |<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission TICKET_VIEW --enable true</code></pre>  |
{: class="simple-tab-table"}
{: caption="Required classic infrastructure permissions" caption-side="top"}
{: #classic-permissions-required}
{: tab-title="Create clusters"}
{: tab-group="Classic infrastructure permissions"}

| Permission | Description | IAM Assign Policy Console | CLI |
|:-----------------|:-----------------|:---------------|:----|
| Access All Virtual | Designate access to all VM worker nodes. Without this permission, a user who creates one cluster might not be able to view the VM worker nodes of another cluster even if the user has IAM access to both clusters. | Classic infrastructure > Devices > Check All virtual servers and Auto virtual server access|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission ACCESS_ALL_GUEST --enable true</code></pre> |
| Access All Hardware | Designate access to all bare metal worker nodes.  Without this permission, a user who creates one cluster might not be able to view the bare metal worker nodes of another cluster even if the user has IAM access to both clusters. | Classic infrastructure > Devices > Check All bare metal servers and Auto bare metal server access|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission ACCESS_ALL_HARDWARE --enable true</code></pre> |
| Add Compute with Public Network Port | Let worker nodes have a port that can be accessible on the public network. | Classic infrastructure > Permissions > Network|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission PUBLIC_NETWORK_COMPUTE --enable true</code></pre> |
| Manage DNS | Set up public load balancer or Ingress networking to expose apps. | Classic infrastructure > Permissions > Services|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission DNS_MANAGE --enable true</code></pre> |
| Edit Hostname/Domain | Set up public load balancer or Ingress networking to expose apps. | Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission HOSTNAME_EDIT --enable true</code></pre> |
| Add IP Addresses | Add IP addresses to public or private subnets that are used for cluster load balancing. | Classic infrastructure > Permissions > Network|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission IP_ADD --enable true</code></pre> |
| Manage Network Subnet Routes | Manage public and private VLANs and subnets that are used for cluster load balancing. | Classic infrastructure > Permissions > Network|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission NETWORK_ROUTE_MANAGE --enable true</code></pre> |
| Manage Port Control | Manage ports that are used for app load balancing. | Classic infrastructure > Permissions > Devices|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission PORT_CONTROL --enable true</code></pre> |
| Manage Certificates (SSL) | Set up certificates that are used for cluster load balancing. | Classic infrastructure > Permissions > Services|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission SECURITY_CERTIFICATE_MANAGE --enable true</code></pre>  |
| View Certificates (SSL) | Set up certificates that are used for cluster load balancing. | Classic infrastructure > Permissions > Services|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission SECURITY_CERTIFICATE_VIEW --enable true</code></pre> |
| Add/Upgrade Storage (Storage Layer) | Create {{site.data.keyword.cloud_notm}} File or Block storage instances to attach as volumes to your apps for persistent storage of data. | Classic infrastructure > Permissions > Account|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission ADD_SERVICE_STORAGE --enable true</code></pre>  |
| Storage Manage | Manage {{site.data.keyword.cloud_notm}} File or Block storage instances that are attached as volumes to your apps for persistent storage of data. | Classic infrastructure > Permissions > Services|<pre class="pre"><code>ibmcloud sl user permission-edit &lt;user_id&gt; --permission NAS_MANAGE --enable true</code></pre> |
{: class="simple-tab-table"}
{: caption="Suggested classic infrastructure permissions" caption-side="top"}
{: #classic-permissions-suggested}
{: tab-title="Other common use cases"}
{: tab-group="Classic infrastructure permissions"}



