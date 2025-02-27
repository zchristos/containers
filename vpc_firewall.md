---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, firewall, ips

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
 


# VPC: Opening required ports and IP addresses in other network firewalls
{: #vpc-firewall}

<img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> This firewall information is specific to VPC clusters. For firewall information for classic clusters, see [Opening required ports and IP addresses in your firewall for classic clusters](/docs/containers?topic=containers-firewall).
{: note}

## Opening ports in a corporate firewall
{: #vpc-corporate}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, you must allow access to run [`ibmcloud`, `ibmcloud ks`, and `ibmcloud cr` commands](#vpc-firewall_bx), [`kubectl` commands](#vpc-firewall_kubectl), and [`calicoctl` commands](#vpc-firewall_calicoctl) from your local system.
{: shortdesc}

### Running `ibmcloud`, `ibmcloud ks`, and `ibmcloud cr` commands from behind a firewall
{: #vpc-firewall_bx}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `ibmcloud`, `ibmcloud ks` and `ibmcloud cr` commands, you must allow TCP access for {{site.data.keyword.cloud_notm}}, {{site.data.keyword.containerlong_notm}}, and {{site.data.keyword.registrylong_notm}}.
{: shortdesc}

1. Allow access to `cloud.ibm.com` on port 443 in your firewall.
2. Verify your connection by logging in to {{site.data.keyword.cloud_notm}} through this API endpoint.
  ```
  ibmcloud login -a https://cloud.ibm.com/
  ```
  {: pre}
3. Allow access to `containers.cloud.ibm.com` on port 443 in your firewall.
4. Verify your connection. If access is configured correctly, ships are displayed in the output.
   ```
   curl https://containers.cloud.ibm.com/v1/
   ```
   {: pre}

   Example output:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}
5. Allow access to the [{{site.data.keyword.registrylong_notm}} regions](/docs/Registry?topic=Registry-registry_overview#registry_regions) that you plan to use on port 443 and 4443 in your firewall. The global registry stores IBM-provided public images, and regional registries store your own private or public images. If your firewall is IP-based, you can see which IP addresses are opened when you allow access to the {{site.data.keyword.registrylong_notm}} regional service endpoints by reviewing [this table](/docs/containers?topic=containers-firewall#firewall_registry).
  * Global registry: `icr.io`
  * AP North: `jp.icr.io`
  * AP South: `au.icr.io`
  * EU Central: `de.icr.io`
  * UK South: `uk.icr.io`
  * US East, US South: `us.icr.io`

6. Verify your connection. The following is an example for the US East and US South regional registry. If access is configured correctly, a message of the day is returned in the output.
   ```
   curl https://us.icr.io/api/v1/messages
   ```
   {: pre}

</br>

### Running `kubectl` commands from behind a firewall
{: #vpc-firewall_kubectl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `kubectl` commands, you must allow TCP access for the cluster.
{: shortdesc}

When a cluster is created, the port in the service endpoint URLs is randomly assigned from within 20000-32767. You can either choose to open port range 20000-32767 for any cluster that might get created or you can choose to allow access for a specific existing cluster.

Before you begin, allow access to [run `ibmcloud ks` commands](#vpc-firewall_bx).

To allow access for a specific cluster:

1. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted. If you have a federated account, include the `--sso` option.

   ```
   ibmcloud login [--sso]
   ```
   {: pre}

2. If the cluster is in a resource group other than `default`, target that resource group. To see the resource group that each cluster belongs to, run `ibmcloud ks cluster ls`. **Note**: You must have at least the [**Viewer** role](/docs/containers?topic=containers-users#platform) for the resource group.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

4. Get the name of your cluster.

   ```
   ibmcloud ks cluster ls
   ```
   {: pre}

5. Retrieve the service endpoint URLs for your cluster.
 * If only the **Private Service Endpoint URL** is populated, get this URL. Your authorized cluster users can access the master through this endpoint on the private network.
 * If both the **Public Service Endpoint URL** and **Private Service Endpoint URL** are populated, get both URLs. Your authorized cluster users can access the master through the public endpoint on the public network or the private endpoint on the private network.

  ```
  ibmcloud ks cluster get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Example output:
  ```
  ...
  Public Service Endpoint URL:    https://c3.<region>.containers.cloud.ibm.com:30426
  Private Service Endpoint URL:   https://c3-private.<region>.containers.cloud.ibm.com:31140
  ...
  ```
  {: screen}

6. Allow access to the service endpoint URLs and ports that you got in the previous step. If your firewall is IP-based, you can see which IP addresses are opened when you allow access to the service endpoint URLs by reviewing [this table](/docs/containers?topic=containers-firewall#master_ips).

7. Verify your connection.
  * If the public cloud service endpoint is enabled:
    ```
    curl --insecure <public_service_endpoint_URL>/version
    ```
    {: pre}

    Example command:
    ```
    curl --insecure https://c3.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Example output:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
    }
    ```
    {: screen}
  * If only the private cloud service endpoint is enabled, you must be in your {{site.data.keyword.cloud_notm}} private network or connect to the private network through a VPN connection to verify your connection to the master. **Note**: You must [expose the master endpoint through a private load balancer](/docs/containers?topic=containers-access_cluster#access_private_se) so that users can access the master through a VPN or {{site.data.keyword.BluDirectLink}} connection.
    ```
    curl --insecure <private_service_endpoint_URL>/version
    ```
    {: pre}

    Example command:
    ```
    curl --insecure https://c3-private.<region>.containers.cloud.ibm.com:31142/version
    ```
    {: pre}

    Example output:
    ```
    {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
    }
    ```
    {: screen}

8. Optional: Repeat these steps for each cluster that you need to expose.

</br>

### Running `calicoctl` commands from behind a firewall
{: #vpc-firewall_calicoctl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `calicoctl` commands, you must allow TCP access for the Calico commands.
{: shortdesc}

Before you begin, allow access to run [`ibmcloud` commands](#vpc-firewall_bx) and [`kubectl` commands](#vpc-firewall_kubectl).

1. Retrieve the IP address from the master URL that you used to allow the [`kubectl` commands](#vpc-firewall_kubectl).

2. Get the port for etcd.

  ```
  kubectl get cm -n kube-system cluster-info -o yaml | grep etcd_host
  ```
  {: pre}

3. Allow access for the Calico policies via the master URL IP address and the etcd port.

<br />

## Allowing traffic from your cluster in other services' firewalls or in on-premises firewalls
{: #vpc-allowlist_workers}

Allow your worker nodes to communicate with services that are protected by firewalls.
{: shortdesc}

For example, you might have services that run inside or outside {{site.data.keyword.cloud_notm}}, or services that run on-premises, that are protected by a firewall. You want to permit incoming network traffic to those services from your cluster. In your service's firewall, you must add the external IP addresses of the public gateways on your cluster's VPC subnets.

If you want to permit egress from your firewall-protected services to your cluster, you must add your worker nodes' private IP addresses or your cluster's VPC subnet CIDRs in your service's firewall. Note that because worker nodes in VPC clusters have only private IP addresses, connections into the VPC cluster worker nodes can only originate from systems that are connected to your IBM Cloud private network.

Before you begin:
1. [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2. Install the `infrastructure-service` CLI plug-in. The prefix for running VPC infrastructure commands is `ibmcloud is`.
    ```
    ibmcloud plugin install infrastructure-service
    ```
    {: pre}

### Allowing ingress from a cluster to another service
{: #vpc-allowlist_workers_ingress}

To permit ingress from your cluster to another service, modify that service's firewall or your on-premises firewall.
{: shortdesc}

1. Get the **Worker Zones** and **VPCs** that your cluster is created in.
  ```
  ibmcloud ks cluster get -c <cluster>
  ```
  {: pre}

  Example output:
  ```
  ...
  Worker Zones:                   us-south-1, us-south-2, us-south-3
  Ingress Subdomain:              vpc-prod.us-south.containers.appdomain.cloud
  Ingress Secret:                 vpc-prod
  Creator:                        -
  Public Service Endpoint URL:    https://c2.us-south.containers.cloud.ibm.com:20267
  Private Service Endpoint URL:   https://c2.private.us-south.containers.cloud.ibm.com:20267
  Pull Secrets:                   enabled in the default namespace
  VPCs:                           ff537d43-a5a4-4b65-9627-17eddfa5237b
  ...
  ```
  {: screen}

2. For the worker zones and VPC that you found, ensure that you [enabled a public gateway on the VPC subnets in each worker zone](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#attach-public-gateway-cli).

3. List the public gateways for the subnets. In the output, for the zones and VPC that your cluster is in, note the gateway **Floating IP** addresses for the subnets.
  ```
  ibmcloud is public-gateways
  ```
  {: pre}

  Example output:
  ```
  ID                                     Name                                       Status      Floating IP      VPC              Zone
  5d308ea5-9f32-43b3-aaae-194d5723a3e5   pgw-b9d45630-c053-11e9-b2f8-79328ce05e7e   available   169.XX.XXX.XX    test-vpc         us-south-1
  f8b95e43-a408-4dc8-a489-ed649fc4cfec   pgw-18a3ebb0-b539-11e9-9838-f3f4efa02374   available   169.XX.XXX.XX    prod             us-south-1
  2ba9a280-fffa-4b0c-bdca-7970f09f9b8a   pgw-73b62bc0-b53a-11e9-9838-f3f4efa02374   available   169.XX.XXX.XX    prod             us-south-2
  057ddef6-631f-4b22-89eb-1e99982a54fa   pgw-64c5cae0-0be2-11ea-8f26-e1565e79a36c   available   52.XX.XXX.XXX    prod             us-south-3
  ```
  {: screen}

4.  Add the public gateway IP addresses to your service's firewall or your on-premises firewall for inbound traffic.
5.  Repeat these steps for each cluster that you want to allow traffic to or from.

### Allowing egress to a cluster from another service
{: #vpc-allowlist_workers_egress}

To permit egress to your cluster from another service, modify that service's firewall or your on-premises firewall.
{: shortdesc}

1. Get the worker node subnets or the worker node IP addresses.
  * **Worker node subnet CIDRs**: If you anticipate changing the number of worker nodes in your cluster frequently, such as if you enable the [cluster autoscaler](/docs/containers?topic=containers-ca), you might not want to update your firewall for each new worker node. Instead, you can add the VPC subnets that the cluster uses. Keep in mind that the VPC subnet might be shared by worker nodes in other clusters.
    1. Get the **Worker Zones** and **VPCs** that your cluster is created in.
      ```
      ibmcloud ks cluster get -c <cluster>
      ```
      {: pre}

      Example output:
      ```
      ...
      Worker Zones:                   us-south-1, us-south-2, us-south-3
      Ingress Subdomain:              vpc-prod.us-south.containers.appdomain.cloud
      Ingress Secret:                 vpc-prod
      Creator:                        -
      Public Service Endpoint URL:    https://c2.us-south.containers.cloud.ibm.com:20267
      Private Service Endpoint URL:   https://c2.private.us-south.containers.cloud.ibm.com:20267
      Pull Secrets:                   enabled in the default namespace
      VPCs:                           ff537d43-a5a4-4b65-9627-17eddfa5237b
      ...
      ```
      {: screen}

    2. For the subnets in the zones and VPC that your cluster is in, note the **Subnet CIDR**.
      ```
      ibmcloud is subnets
      ```
      {: pre}

      Example output:
      ```
      ID                                     Name             Status      Subnet CIDR        Addresses   ACL                                                          Public Gateway                             VPC              Zone
      5f5787a4-f560-471b-b6ce-20067ac93439   vpc-prod-dal1    available   10.240.0.0/24      183/256     allow-all-network-acl-ff537d43-a5a4-4b65-9627-17eddfa5237b   -                                          prod             us-south-1
      e3c19786-1c54-4248-86ca-e60aab74ed62   vpc-prod-dal2    available   10.240.64.0/24     183/256     allow-all-network-acl-ff537d43-a5a4-4b65-9627-17eddfa5237b   -                                          prod             us-south-2
      2930a068-51cc-4eca-807b-3f296d0891b4   vpc-prod-dal3    available   10.240.128.0/24    249/256     allow-all-network-acl-ff537d43-a5a4-4b65-9627-17eddfa5237b   -                                          prod             us-south-3
      ```
      {: screen}

  * **Individual worker node IP addresses**: If you have a small number of worker nodes that run only one app and do not need to scale, or if you want to add only one worker node, list all the worker nodes in your cluster and note the **Primary IP** addresses. Only these worker nodes are added. If you delete the worker nodes or add worker nodes to the cluster, you must update your firewall accordingly.
    ```
    ibmcloud ks worker ls --cluster <cluster_name_or_ID>
    ```
    {: pre}

2.  Add the subnet CIDRs or individual worker node IP addresses to your service's firewall or your on-premises firewall for outbound traffic.
3.  Repeat these steps for each cluster that you want to allow traffic to or from.
