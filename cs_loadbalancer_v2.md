---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, lb2.0, nlb

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
 


# Classic: Setting up DSR load balancing with an NLB 2.0
{: #loadbalancer-v2}

<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Version 2.0 NLBs can be created in classic clusters only, and cannot be created in VPC clusters. To load balance in VPC clusters, see [Exposing apps with load balancers for VPC](/docs/containers?topic=containers-vpc-lbaas).
{: note}

Expose a port and use a portable IP address for a Layer 4 network load balancer (NLB) to expose a containerized app. For more information about version 2.0 NLBs, see [Components and architecture of an NLB 2.0](/docs/containers?topic=containers-loadbalancer-about#planning_ipvs).
{: shortdesc}

## Prerequisites
{: #ipvs_provision}

You cannot update an existing version 1.0 NLB to 2.0. You must create a new NLB 2.0. Note that you can run version 1.0 and 2.0 NLBs simultaneously in a cluster.
{: shortdesc}

Before you create an NLB 2.0, you must complete the following prerequisite steps.

1. [Update your cluster's master and worker nodes](/docs/containers?topic=containers-update) to Kubernetes version 1.12 or later.

2. To allow your NLB 2.0 to forward requests to app pods in multiple zones, open a support case to request capacity aggregation for your VLANs. This configuration setting does not cause any network disruptions or outages.
    1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/).
    2. From the menu bar, click **Support**, click the **Manage cases** tab, and click **Create new case**.
    3. In the case fields, enter the following:
       * For type of support, select **Technical**.
       * For offering, select **Containers**.
       * For subject, enter **Public and private VLAN network question**
    4. Add the following information to the description: "Please set up the network to allow capacity aggregation on the public and private VLANs associated with my account.". Note that if you want to allow capacity aggregation on specific VLANs, such as the public VLANs for one cluster only, you can specify those VLAN IDs in the description.
    5. Click **Submit**.

3. Enable a [Virtual Router Function (VRF)](/docs/account?topic=account-vrf-service-endpoint#vrf) for your IBM Cloud infrastructure account. To enable VRF, see [Enabling VRF](/docs/account?topic=account-vrf-service-endpoint#vrf). To check whether a VRF is already enabled, use the `ibmcloud account show` command. If you cannot or do not want to enable VRF, enable [VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning). When a VRF or VLAN spanning is enabled, the NLB 2.0 can route packets to various subnets in the account.

4. If you use [Calico pre-DNAT network policies](/docs/containers?topic=containers-network_policies#block_ingress) to manage traffic to an NLB 2.0, you must add the `applyOnForward: true` and `doNotTrack: true` fields to and remove the `preDNAT: true` from the `spec` section in the policies. `applyOnForward: true` ensures that the Calico policy is applied to the traffic as it is encapsulated and forwarded. `doNotTrack: true` ensures that the worker nodes can use DSR to return a response packet directly to the client without needing the connection to be tracked. For example, if you use a Calico policy to allow traffic from only specific IP addresses to your NLB IP address, the policy looks similar to the following:
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: allowlist
    spec:
      applyOnForward: true
      doNotTrack: true
      ingress:
      - action: Allow
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source:
          nets:
          - <client_address>/32
      selector: ibm.role=='worker_public'
      order: 500
      types:
      - Ingress
    ```
    {: screen}

Next, you can follow the steps in [Setting up an NLB 2.0 in a multizone cluster](#ipvs_multi_zone_config) or [in a single-zone cluster](#ipvs_single_zone_config).

<br />

## Setting up an NLB 2.0 in a multizone cluster
{: #ipvs_multi_zone_config}

**Before you begin**:

* **Important**: Complete the [NLB 2.0 prerequisites](#ipvs_provision).
* To create public NLBs in multiple zones, at least one public VLAN must have portable subnets available in each zone. To create private NLBs in multiple zones, at least one private VLAN must have portable subnets available in each zone. You can add subnets by following the steps in [Configuring subnets for clusters](/docs/containers?topic=containers-subnets).
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role](/docs/containers?topic=containers-users#platform) for the `default` namespace.
* Ensure you have the required number of worker nodes:
  * Classic clusters: If you restrict network traffic to edge worker nodes, ensure that at least two [edge worker nodes](/docs/containers?topic=containers-edge#edge) are enabled in each zone so that NLBs deploy uniformly.
  * Gateway-enabled classic clusters: Ensure that at least two gateway worker nodes in the `gateway` worker pool are enabled in each zone so that NLBs deploy uniformly.

To set up an NLB 2.0 in a multizone cluster:
1.  [Deploy your app to the cluster](/docs/containers?topic=containers-deploy_app#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file. This custom label identifies all pods where your app runs to include them in the load balancing.

2.  Create a load balancer service for the app that you want to expose to the public internet or a private network.
  1. Create a service configuration file that is named, for example, `myloadbalancer.yaml`.
  2. Define a load balancer service for the app that you want to expose. You can specify a zone, a VLAN, and an IP address.

      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan_id>"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler: "<algorithm>"
      spec:
        type: LoadBalancer
        selector:
          <selector_key>: <selector_value>
        ports:
         - protocol: TCP
           port: 8080
        loadBalancerIP: <IP_address>
        externalTrafficPolicy: Local
      ```
      {: codeblock}

      <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
      <caption>Understanding the YAML file components</caption>
      <col width="50%">
      <thead>
      <th>Parameter</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:</code>
        <td>Annotation to specify a <code>private</code> or <code>public</code> load balancer.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-zone:</code>
        <td>Annotation to specify the zone that the load balancer service deploys to. To see zones, run <code>ibmcloud ks zone ls</code>.</td>
      </tr>
      <tr>
        <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
        <td>Annotation to specify a VLAN that the load balancer service deploys to. To see VLANs, run <code>ibmcloud ks vlan ls --zone <zone></code>.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"</code>
        <td>Annotation to specify a version 2.0 load balancer.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler:</code>
        <td>Optional: Annotation to specify the scheduling algorithm. Accepted values are <code>"rr"</code> for Round Robin (default) or <code>"sh"</code> for Source Hashing. For more information, see [2.0: Scheduling algorithms](#scheduling).</td>
      </tr>
      <tr>
        <td><code>selector</code></td>
        <td>The label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) that you used in the <code>spec.template.metadata.labels</code> section of your app deployment YAML.</td>
      </tr>
      <tr>
        <td><code>port</code></td>
        <td>The port that the service listens on.</td>
      </tr>
      <tr>
        <td><code>loadBalancerIP</code></td>
        <td>Optional: To create a private NLB or to use a specific portable IP address for a public NLB, specify the IP address that you want to use. The IP address must be in the zone and VLAN that you specify in the annotations. If you do not specify an IP address:<ul><li>If your cluster is on a public VLAN, a portable public IP address is used. Most clusters are on a public VLAN.</li><li>If your cluster is on a private VLAN only, a portable private IP address is used.</li></ul></td>
      </tr>
      <tr>
        <td><code>externalTrafficPolicy: Local</code></td>
        <td>Set to <code>Local</code>.</td>
      </tr>
      </tbody></table>

      Example configuration file to create an NLB 2.0 service in `dal12` that uses the Round Robin scheduling algorithm:

      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "dal12"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler: "rr"
      spec:
        type: LoadBalancer
        selector:
          app: nginx
        ports:
         - protocol: TCP
           port: 8080
        externalTrafficPolicy: Local
      ```
      {: codeblock}

  3. Optional: Make your NLB service available to only a limited range of IP addresses by specifying the IPs in the `spec.loadBalancerSourceRanges` field. `loadBalancerSourceRanges` is implemented by `kube-proxy` in your cluster via Iptables rules on worker nodes. For more information, see the [Kubernetes documentation](https://v1-17.docs.kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/){: external}.

  4. Create the service in your cluster.

      ```
      kubectl apply -f myloadbalancer.yaml
      ```
      {: pre}

3. Verify that the NLB service was created successfully. It might take a few minutes for the NLB service to be created properly and for the app to be available.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    Example CLI output:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Zone:                   dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your NLB service.

4.  If you created a public NLB, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the NLB and port.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. To achieve high availability, repeat the steps 2 - 4 to add an NLB 2.0 in each zone where you have app instances.

6. Optional: An NLB service also makes your app available over the service's NodePorts. [NodePorts](/docs/containers?topic=containers-nodeport) are accessible on every public and private IP address for every node within the cluster. To block traffic to NodePorts while you are using an NLB service, see [Controlling inbound traffic to network load balancer (NLB) or NodePort services](/docs/containers?topic=containers-network_policies#block_ingress).

Next, you can [register an NLB subdomain](/docs/containers?topic=containers-loadbalancer_hostname).

<br />

## Setting up an NLB 2.0 in a single-zone cluster
{: #ipvs_single_zone_config}

**Before you begin**:

* **Important**: Complete the [NLB 2.0 prerequisites](#ipvs_provision).
* You must have an available portable public or private IP address to assign to the NLB service. For more information, see [Configuring subnets for clusters](/docs/containers?topic=containers-subnets).
* Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role](/docs/containers?topic=containers-users#platform) for the `default` namespace.

To create an NLB 2.0 service in a single-zone cluster:

1.  [Deploy your app to the cluster](/docs/containers?topic=containers-deploy_app#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app runs so that they can be included in the load balancing.
2.  Create a load balancer service for the app that you want to expose to the public internet or a private network.
    1.  Create a service configuration file that is named, for example, `myloadbalancer.yaml`.

    2.  Define a load balancer 2.0 service for the app that you want to expose.
        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
            service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan: "<vlan_id>"
            service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler: "<algorithm>"
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
          externalTrafficPolicy: Local
        ```
        {: codeblock}

        <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
        <caption>Understanding the YAML file components</caption>
        <col width="50%">
        <thead>
        <th>Parameter</th>
        <th>Description</th>
        </thead>
        <tbody>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation to specify a <code>private</code> or <code>public</code> load balancer.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan:`
          <td>Optional: Annotation to specify a VLAN that the load balancer service deploys to. To see VLANs, run <code>ibmcloud ks vlan ls --zone <zone></code>.</td>
        </tr>
        <tr>
          <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "ipvs"</code>
          <td>Annotation to specify a load balancer 2.0.</td>
        </tr>
        <tr>
          <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler:</code>
          <td>Optional: Annotation to specify a scheduling algorithm. Accepted values are <code>"rr"</code> for Round Robin (default) or <code>"sh"</code> for Source Hashing. For more information, see [2.0: Scheduling algorithms](#scheduling).</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>The label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) that you used in the <code>spec.template.metadata.labels</code> section of your app deployment YAML.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>The port that the service listens on.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>Optional: To create a private NLB or to use a specific portable IP address for a public NLB, specify the IP address that you want to use. The IP address must be on the VLAN that you specify in the annotations. If you do not specify an IP address:<ul><li>If your cluster is on a public VLAN, a portable public IP address is used. Most clusters are on a public VLAN.</li><li>If your cluster is on a private VLAN only, a portable private IP address is used.</li></ul></td>
        </tr>
        <tr>
          <td><code>externalTrafficPolicy: Local</code></td>
          <td>Set to <code>Local</code>.</td>
        </tr>
        </tbody></table>

    3.  Optional: Make your NLB service available to only a limited range of IP addresses by specifying the IPs in the `spec.loadBalancerSourceRanges` field. `loadBalancerSourceRanges` is implemented by `kube-proxy` in your cluster via Iptables rules on worker nodes. For more information, see the [Kubernetes documentation](https://v1-17.docs.kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/){: external}.

    4.  Create the service in your cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

3.  Verify that the NLB service was created successfully. It might take a few minutes for the service to be created and for the app to be available.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    Example CLI output:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your NLB service.

4.  If you created a public NLB, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the NLB and port.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. Optional: An NLB service also makes your app available over the service's NodePorts. [NodePorts](/docs/containers?topic=containers-nodeport) are accessible on every public and private IP address for every node within the cluster. To block traffic to NodePorts while you are using an NLB service, see [Controlling inbound traffic to network load balancer (NLB) or NodePort services](/docs/containers?topic=containers-network_policies#block_ingress).

Next, you can [register an NLB subdomain](/docs/containers?topic=containers-loadbalancer_hostname).

<br />

## Scheduling algorithms
{: #scheduling}

Scheduling algorithms determine how an NLB 2.0 assigns network connections to your app pods. As client requests arrive to your cluster, the NLB routes the request packets to worker nodes based on the scheduling algorithm. To use a scheduling algorithm, specify its Keepalived short name in the scheduler annotation of your NLB service configuration file: `service.kubernetes.io/ibm-load-balancer-cloud-provider-ipvs-scheduler: "rr"`. Check the following lists to see which scheduling algorithms are supported in {{site.data.keyword.containerlong_notm}}. If you do not specify a scheduling algorithm, the Round Robin algorithm is used by default. For more information, see the [Keepalived documentation](https://www.keepalived.org/doc/scheduling_algorithms.html){: external}.
{: shortdesc}

### Supported scheduling algorithms
{: #scheduling_supported}

<dl>
<dt>Round Robin (<code>rr</code>)</dt>
<dd>The NLB cycles through the list of app pods when routing connections to worker nodes, treating each app pod equally. Round Robin is the default scheduling algorithm for version 2.0 NLBs.</dd>
<dt>Source Hashing (<code>sh</code>)</dt>
<dd>The NLB generates a hash key based on the source IP address of the client request packet. The NLB then looks up the hash key in a statically assigned hash table, and routes the request to the app pod that handles hashes of that range. This algorithm ensures that requests from a particular client are always directed to the same app pod.</br>**Note**: Kubernetes uses Iptables rules, which cause requests to be sent to a random pod on the worker. To use this scheduling algorithm, you must ensure that no more than one pod of your app is deployed per worker node. For example, if each pod has the label <code>run=&lt;app_name&gt;</code>, add the following anti-affinity rule to the <code>spec</code> section of your app deployment:</br>
<pre class="codeblock">
<code>
    spec:
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: run
                  operator: In
                  values:
                  - <APP_NAME>
              topologyKey: kubernetes.io/hostname</code></pre>

You can find the complete example in [this IBM Cloud deployment pattern blog](https://www.ibm.com/cloud/blog/ibm-cloud-kubernetes-service-deployment-patterns-4-multi-zone-cluster-app-exposed-via-loadbalancer-aggregating-whole-region-capacity){: external}.</dd>
</dl>

### Unsupported scheduling algorithms
{: #scheduling_unsupported}

<dl>
<dt>Destination Hashing (<code>dh</code>)</dt>
<dd>The destination of the packet, which is the NLB IP address and port, is used to determine which worker node handles the incoming request. However, the IP address and port for NLBs in {{site.data.keyword.containerlong_notm}} don't change. The NLB is forced to keep the request within the same worker node that it is on, so only app pods on one worker handle all incoming requests.</dd>
<dt>Dynamic connection counting algorithms</dt>
<dd>The following algorithms depend on dynamic counting of connections between clients and NLBs. However, because direct service return (DSR) prevents NLB 2.0 pods from being in the return packet path, NLBs don't track established connections.<ul>
<li>Least Connection (<code>lc</code>)</li>
<li>Locality-Based Least Connection (<code>lblc</code>)</li>
<li>Locality-Based Least Connection with Replication (<code>lblcr</code>)</li>
<li>Never Queue (<code>nq</code>)</li>
<li>Shortest Expected Delay (<code>seq</code>)</li></ul></dd>
<dt>Weighted pod algorithms</dt>
<dd>The following algorithms depend on weighted app pods. However, in {{site.data.keyword.containerlong_notm}}, all app pods are assigned equal weight for load balancing.<ul>
<li>Weighted Least Connection (<code>wlc</code>)</li>
<li>Weighted Round Robin (<code>wrr</code>)</li></ul></dd></dl>
