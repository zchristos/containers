---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks

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
 


# Storing data on classic IBM Cloud {{site.data.keyword.filestorage_short}}
{: #file_storage}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.filestorage_short}} is persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}} that you can add to your apps by using Kubernetes persistent volumes (PVs). You can choose between predefined storage tiers with GB sizes and IOPS that meet the requirements of your workloads. To find out if {{site.data.keyword.cloud_notm}} {{site.data.keyword.filestorage_short}} is the right storage option for you, see [Choosing a storage solution](/docs/containers?topic=containers-storage_planning#choose_storage_solution). For pricing information, see [Pricing](https://www.ibm.com/cloud/file-storage/pricing){: external}.
{: shortdesc}

{{site.data.keyword.filestorage_full_notm}} is available only in classic {{site.data.keyword.containerlong_notm}} clusters, and is not supported for VPC clusters. NFS {{site.data.keyword.filestorage_short}} instances are specific to a single zone. If you have a multizone cluster, consider [multizone persistent storage options](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
{: important}

## Quickstart for {{site.data.keyword.cloud_notm}} {{site.data.keyword.filestorage_short}}
{: #file_qs}

In this quickstart guide, you create a 24Gi endurance {{site.data.keyword.filestorage_short}} volume in your cluster by creating a PVC to dynamically provision the volume. Then, you create an app deployment that mounts your PVC.
{: shortdesc}

First time using {{site.data.keyword.filestorage_short}} in your cluster? Come back here once you are familiar with the [{{site.data.keyword.filestorage_short}}](#file_predefined_storageclass) configurations.
{: tip}

1. Create a file for your PVC and name it `pvc.yaml`.

  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: silver-pvc
    labels:
      billingType: hourly
      region: # Example: us-south
      zone: # Example: dal13
  spec:
    accessModes:
      - ReadWriteMany
    resources:
      requests:
        storage: 24Gi
    storageClassName: ibmc-file-silver
  ```
  {: codeblock}

2. Create the PVC in your cluster.
  ```sh
  kubectl apply -f pvc.yaml
  ```
  {: pre}

3. After your `silver-pvc` PVC is bound, create an app deployment that uses your PVC. Create a file for your deployment and name it `deployment.yaml`.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
      labels:
        app:
    spec:
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - image: # Your contanerized app image.
            name: my-container
            volumeMounts:
            - name: my-volume
              mountPath: /mount-path
          volumes:
          - name: my-volume
            persistentVolumeClaim:
              claimName: silver-pvc
    ```
    {: codeblock}

3. Create the deployment in your cluster.
  ```sh
  kubectl apply -f deployment.yaml
  ```
  {: pre}

For more information, see:
  * [Adding {{site.data.keyword.filestorage_short}} to apps](#add_file).
  * [Storage class reference](#file_storageclass_reference).
  * [Custom storage classes](#file_custom_storageclass).

## Deciding on the {{site.data.keyword.filestorage_short}} configuration
{: #file_predefined_storageclass}

{{site.data.keyword.containerlong}} provides pre-defined storage classes for {{site.data.keyword.filestorage_short}} that you can use to provision {{site.data.keyword.filestorage_short}} with a specific configuration.
{: shortdesc}

Every storage class specifies the type of {{site.data.keyword.filestorage_short}} that you provision, including available size, IOPS, file system, and the retention policy.  

After you provision a specific type of storage by using a storage class, you cannot change the type, or retention policy for the storage device. However, you can [change the size and the IOPS](#file_change_storage_configuration) if you want to increase your storage capacity and performance. To change the type and retention policy for your storage, you must [create a new storage instance and copy the data](/docs/containers?topic=containers-kube_concepts#update_storageclass) from the old storage instance to your new one.
{: important}

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To decide on a storage configuration:

1. List available storage classes in {{site.data.keyword.containerlong}}.
   ```
   kubectl get storageclasses | grep file
   ```
   {: pre}

   Example output:
   ```
   $ kubectl get storageclasses
   NAME                         TYPE
   ibmc-file-bronze (default)   ibm.io/ibmc-file
   ibmc-file-custom             ibm.io/ibmc-file
   ibmc-file-gold               ibm.io/ibmc-file
   ibmc-file-retain-bronze      ibm.io/ibmc-file
   ibmc-file-retain-custom      ibm.io/ibmc-file
   ibmc-file-retain-gold        ibm.io/ibmc-file
   ibmc-file-retain-silver      ibm.io/ibmc-file
   ibmc-file-silver             ibm.io/ibmc-file
   ```
   {: screen}

2. Review the configuration of a storage class.
  ```
  kubectl describe storageclass <storageclass_name>
  ```
  {: pre}

   For more information about each storage class, see the [storage class reference](#file_storageclass_reference). If you do not find what you are looking for, consider creating your own customized storage class. To get started, check out the [customized storage class samples](#file_custom_storageclass).
   {: tip}

3. Choose the [type of {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-getting-started#prereqs) that you want to provision.
   - **Bronze, silver, and gold storage classes:** These storage classes provision Endurance storage. Endurance storage lets you choose the size of the storage in gigabytes at predefined IOPS tiers.
   - **Custom storage class:** This storage class provisions Performance storage. With performance storage, you have more control over the size of the storage and the IOPS.

4. Choose the size and IOPS for your {{site.data.keyword.filestorage_short}}. The size and the number of IOPS define the total number of IOPS (input/ output operations per second) that serves as an indicator for how fast your storage is. The more total IOPS your storage has, the faster it processes read and write operations.
   - **Bronze, silver, and gold storage classes:** These storage classes come with a fixed number of IOPS per gigabyte and are provisioned on SSD hard disks. The total number of IOPS depends on the size of the storage that you choose. You can select any whole number of gigabyte within the allowed size range, such as 20 Gi, 256 Gi, or 11854 Gi. To determine the total number of IOPS, you must multiply the IOPS with the selected size. For example, if you select a 1000Gi {{site.data.keyword.filestorage_short}} size in the silver storage class that comes with 4 IOPS per GB, your storage has a total of 4000 IOPS.
     <table summary="The columns are read from left to right. The first column has the storage class. The second column has the IOPS per gigabyte for the storage class. The third column has the size range in gigabytes for the storage class.">
         <caption>Table of storage class size ranges and IOPS per gigabyte</caption>
         <thead>
         <th>Storage class</th>
         <th>IOPS per gigabyte</th>
         <th>Size range in gigabytes</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Silver</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Gold</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **Custom storage class:** When you choose this storage class, you have more control over the size and IOPS that you want. For the size, you can select any whole number of gigabyte within the allowed size range. The size that you choose determines the IOPS range that is available to you. You can choose an IOPS that is a multiple of 100 that is in the specified range. The IOPS that you choose is static and does not scale with the size of the storage. For example, if you choose 40Gi with 100 IOPS, your total IOPS remains 100. </br></br> The IOPS to gigabyte ratio also determines the type of hard disk that is provisioned for you. For example, if you have 500Gi at 100 IOPS, your IOPS to gigabyte ratio is 0.2. Storage with a ratio of less than or equal to 0.3 is provisioned on SATA hard disks. If your ratio is greater than 0.3, then your storage is provisioned on SSD hard disks.  
     <table summary="The columns are read from left to right. The first column has the size range in gigabytes for the custom storage class. The second column has the IOPS range in multiples of 100 that are supported for the size range for the custom storage class.">
         <caption>Table of custom storage class size ranges and IOPS</caption>
         <thead>
         <th>Size range in gigabytes</th>
         <th>IOPS range in multiples of 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. Choose if you want to keep your data after the cluster or the persistent volume claim (PVC) is deleted.
   - If you want to keep your data, then choose a `retain` storage class. When you delete the PVC, only the PVC is deleted. The PV, the physical storage device in your IBM Cloud infrastructure account, and your data still exist. To reclaim the storage and use it in your cluster again, you must remove the PV and follow the steps for [using existing {{site.data.keyword.filestorage_short}}](#existing_file).
   - If you want the PV, the data, and your physical {{site.data.keyword.filestorage_short}} device to be deleted when you delete the PVC, choose a storage class without `retain`.

6. Choose if you want to be billed hourly or monthly. Check the [pricing](https://www.ibm.com/cloud/file-storage/pricing){: external} for more information. By default, all {{site.data.keyword.filestorage_short}} devices are provisioned with an hourly billing type.
   If you choose a monthly billing type, when you remove the persistent storage, you still pay the monthly charge for it, even if you used it only for a short amount of time.
   {: note}

<br />


## Adding {{site.data.keyword.filestorage_short}} to apps
{: #add_file}

Create a persistent volume claim (PVC) to [dynamically provision](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) {{site.data.keyword.filestorage_short}} for your cluster. Dynamic provisioning automatically creates the matching persistent volume (PV) and orders the physical storage device in your IBM Cloud infrastructure account.
{: shortdesc}

Before you begin:
- If you have a firewall, [allow egress access](/docs/containers?topic=containers-firewall#vyatta_firewall) for the IBM Cloud infrastructure IP ranges of the zones that your clusters are in so that you can create PVCs.
- [Decide on a pre-defined storage class](#file_predefined_storageclass) or create a [customized storage class](#file_custom_storageclass).

Looking to deploy {{site.data.keyword.filestorage_short}} in a stateful set? See [Using {{site.data.keyword.filestorage_short}} in a stateful set](#file_statefulset) for more information.
{: tip}

To add {{site.data.keyword.filestorage_short}}:

1.  Create a configuration file to define your persistent volume claim (PVC) and save the configuration as a `.yaml` file.

    - **Example for bronze, silver, gold storage classes**:
       The following `.yaml` file creates a claim that is named `mypvc` of the `"ibmc-file-silver"` storage class, billed `"monthly"`, with a gigabyte size of `24Gi`.

       ```yaml
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "monthly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
         storageClassName: ibmc-file-silver
       ```
       {: codeblock}

    -  **Example for using the custom storage class**:
       The following `.yaml` file creates a claim that is named `mypvc` of the storage class `ibmc-file-retain-custom`, billed `"hourly"`, with a gigabyte size of `45Gi` and IOPS of `"300"`.

       ```yaml
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         labels:
           billingType: "hourly"
           region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
         storageClassName: ibmc-file-retain-custom
       ```
       {: codeblock}

       <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
       <caption>Understanding the YAML file components</caption>
       <thead>
        <th>Component</th>
        <th>Description</th>
      </thead>
       <tbody>
       <tr>
       <td><code>name</code></td>
       <td>Enter the name of the PVC.</td>
       </tr>
       <tr>
         <td><code>billingType</code></td>
          <td>Specify the frequency for which your storage bill is calculated, "monthly" or "hourly". If you do not specify a billing type, the storage is provisioned with an hourly billing type. </td>
       </tr>
       <tr>
       <td><code>region</code></td>
       <td>Optional: Specify the region where you want to provision your {{site.data.keyword.filestorage_short}}. To connect to your storage, create the storage in the same region that your cluster is in. If you specify the region, you must also specify a zone. If you do not specify a region, or the specified region is not found, the storage is created in the same region as your cluster.
       </br></br>To get the region for your cluster, run `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` and look for the region prefix in the **Master URL**, such as `eu-de` in `https://c2.eu-de.containers.cloud.ibm.com:11111`.
       </br></br><strong>Tip: </strong>Instead of specifying the region and zone in the PVC, you can also specify these values in a [customized storage class](#file_multizone_yaml). Then, use your storage class in the <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> section of your PVC. If the region and zone are specified in the storage class and the PVC, the values in the PVC take precedence. </td>
       </tr>
       <tr>
       <td><code>zone</code></td>
       <td>Optional: Specify the zone where you want to provision your {{site.data.keyword.filestorage_short}}. To use your storage in an app, create the storage in the same zone that your worker node is in. To view the zone of your worker node, run <code>ibmcloud ks worker ls --cluster &lt;cluster_name_or_ID&gt;</code> and review the <strong>Zone</strong> column of your CLI output. If you specify the zone, you must also specify a region. If you do not specify a zone or the specified zone is not found in a multizone cluster, the zone is selected on a round-robin basis. </br></br><strong>Tip: </strong>Instead of specifying the region and zone in the PVC, you can also specify these values in a [customized storage class](#file_multizone_yaml). Then, use your storage class in the <code>metadata.annotations.volume.beta.kubernetes.io/storage-class</code> section of your PVC. If the region and zone are specified in the storage class and the PVC, the values in the PVC take precedence.
</td>
       </tr>
       <tr>
       <td><code>accessMode</code></td>
       <td>Specify one of the following options: <ul><li><strong>ReadWriteMany: </strong>The PVC can be mounted by multiple pods. All pods can read from and write to the volume. </li><li><strong>ReadOnlyMany: </strong>The PVC can be mounted by multiple pods. All pods have read-only access. <li><strong>ReadWriteOnce: </strong>The PVC can be mounted by one pod only. This pod can read from and write to the volume. </li></ul></td>
       </tr>
       <tr>
       <td><code>storage</code></td>
       <td>Enter the size of the {{site.data.keyword.filestorage_short}}, in gigabytes (Gi). After your storage is provisioned, you cannot change the size of your {{site.data.keyword.filestorage_short}}. Make sure to specify a size that matches the amount of data that you want to store.</td>
       </tr>
       <tr>
       <td><code>iops</code></td>
       <td>This option is available for the custom storage classes only (`ibmc-file-custom / ibmc-file-retain-custom`). Specify the total IOPS for the storage, selecting a multiple of 100 within the allowable range. If you choose an IOPS other than one that is listed, the IOPS is rounded up.</td>
       </tr>
       <tr>
       <td><code>storageClassName</code></td>
       <td>The name of the storage class that you want to use to provision {{site.data.keyword.filestorage_short}}. You can choose to use one of the [IBM-provided storage classes](#file_storageclass_reference) or [create your own storage class](#file_custom_storageclass). </br> If you do not specify a storage class, the PV is created with the default storage class <code>ibmc-file-bronze</code>. </br></br>**Tip:** Want to set your own default? See [Changing the default storage class](/docs/containers?topic=containers-kube_concepts#default_storageclass).</td>
       </tr>
       </tbody></table>

    If you want to use a customized storage class, create your PVC with the corresponding storage class name, a valid IOPS and size.   
    {: tip}

2.  Create the PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  Verify that your PVC is created and bound to the PV. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Example output:

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #file_app_volume_mount}To mount the storage to your deployment, create a configuration `.yaml` file and specify the PVC that binds the PV.

    If you have an app that requires a non-root user to write to the persistent storage, or an app that requires that the mount path is owned by the root user, see [Adding non-root user access to NFS {{site.data.keyword.filestorage_short}}](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot) or [Enabling root permission for NFS {{site.data.keyword.filestorage_short}}](/docs/containers?topic=containers-cs_troubleshoot_storage#nonroot).
    {: tip}

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
    <caption>Understanding the YAML file components</caption>
    <thead>
      <th>Component</th>
      <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code>app</code></td>
    <td>In the metadata section, enter a label for the deployment.</td>
      </tr>
      <tr>
        <td><code>matchLabels.app</code> <br/> <code>labels.app</code></td>
        <td>In the spec selector and template metadata sections, enter label for your app.</td>
      </tr>
    <tr>
    <td><code>image</code></td>
    <td>The name of the container image that you want to use. To list available images in your {{site.data.keyword.registrylong_notm}} account, run `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code>name</code></td>
    <td>The name of the container that you want to deploy to your cluster.</td>
    </tr>
    <tr>
    <td><code>mountPath</code></td>
    <td>In the container volume mounts section, enter the absolute path of the directory to where the volume is mounted inside the container. Data that is written to the mount path is stored under the <code>root</code> directory in your physical {{site.data.keyword.filestorage_short}} instance. If you want to share a volume between different apps, you can specify [volume sub paths ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) for each of your apps.  </td>
    </tr>
    <tr>
    <td><code>name</code></td>
    <td>In the container volume mounts section, enter the name of the volume to mount to your pod.</td>
    </tr>
    <tr>
    <td><code>name</code></td>
    <td>In the volumes section, enter the name of the volume to mount to your pod. Typically this name is the same as <code>volumeMounts.name</code>.</td>
    </tr>
    <tr>
    <td><code>claimName</code></td>
    <td>In the volumes persistent volume claim section, enter the name of the PVC that binds the PV that you want to use. </td>
    </tr>
    </tbody></table>

5.  Create the deployment.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  Verify that the PV is successfully mounted.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     The mount point is in the **Volume Mounts** field and the volume is in the **Volumes** field.

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

<br />

## Using existing {{site.data.keyword.filestorage_short}} in your cluster
{: #existing_file}

If you have an existing physical storage device that you want to use in your cluster, you can manually create the PV and PVC to [statically provision](/docs/containers?topic=containers-kube_concepts#static_provisioning) the storage.
{: shortdesc}

Before you begin:
- Make sure that you have at least one worker node that exists in the same zone as your existing {{site.data.keyword.filestorage_short}} instance.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

### Step 1: Preparing your existing storage.
{: #existing-file-1}

Before you can start to mount your existing storage to an app, you must retrieve all necessary information for your PV and prepare the storage to be accessible in your cluster.  
{: shortdesc}

**For storage that was provisioned with a `retain` storage class:**

If you provisioned storage with a `retain` storage class and you remove the PVC, the PV and the physical storage device are not automatically removed. To reuse the storage in your cluster, you must remove the remaining PV first.

To use existing storage in a different cluster than the one where you provisioned it, follow the steps for [storage that was created outside of the cluster](#external_storage) to add the storage to the subnet of your worker node.
{: tip}

1. List existing PVs.
   ```
   kubectl get pv
   ```
   {: pre}

   Look for the PV that belongs to your persistent storage. The PV is in a `released` state.

2. Get the details of the PV.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

3. Note the `CapacityGb`, `storageClass`, `failure-domain.beta.kubernetes.io/region`, `failure-domain.beta.kubernetes.io/zone`, `server`, and `path`.

4. Remove the PV.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

5. Verify that the PV is removed.
   ```
   kubectl get pv
   ```
   {: pre}

</br>

**For persistent storage that was provisioned outside the cluster:**

If you want to use existing storage that you provisioned earlier, but never used in your cluster before, you must make the storage available in the same subnet as your worker nodes.

1.  {: #external_storage}From the [IBM Cloud infrastructure portal](https://cloud.ibm.com/classic){: external}, click **Storage**.
2.  Click **{{site.data.keyword.filestorage_short}}** and from the **Actions** menu, select **Authorize Host**.
3.  Select **Subnets**.
4.  From the drop-down list, select the private VLAN subnet that your worker node is connected to. To find the subnet of your worker node, run `ibmcloud ks worker ls --cluster <cluster_name>` and compare the `Private IP` of your worker node with the subnet that you found in the drop-down list.
5.  Click **Submit**.
6.  Click the name of the {{site.data.keyword.filestorage_short}}.
7.  Note the `Mount Point`, the `size`, and the `Location` field. The `Mount Point` field is displayed as `<nfs_server>:<file_storage_path>`.

### Step 2: Creating a persistent volume (PV) and a matching persistent volume claim (PVC)
{: #existing-file-2}

1.  Create a storage configuration file for your PV. Include the values that you retrieved earlier.

    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
     labels:
        failure-domain.beta.kubernetes.io/region: <region>
        failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
     capacity:
       storage: "<size>"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "<nfs_server>"
       path: "<file_storage_path>"
    ```
    {: codeblock}

    <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
    <caption>Understanding the YAML file components</caption>
    <thead>
      <th>Component</th>
      <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Enter the name of the PV object to create.</td>
    </tr>
    <tr>
    <td><code>labels</code></td>
    <td>Enter the region and the zone that you retrieved earlier. You must have at least one worker node in the same region and zone as your persistent storage to mount the storage in your cluster.
    </tr>
    <tr>
    <td><code>storage</code></td>
    <td>Enter the storage size of the existing NFS file share that you retrieved earlier. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB), and the size must match the size of the existing file share.</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Specify one of the following options: <ul><li><strong>ReadWriteMany: </strong>The PVC can be mounted by multiple pods. All pods can read from and write to the volume. </li><li><strong>ReadOnlyMany: </strong>The PVC can be mounted by multiple pods. All pods have read-only access. <li><strong>ReadWriteOnce: </strong>The PVC can be mounted by one pod only. This pod can read from and write to the volume. </li></ul></td>
    </tr>
    <tr>
    <td><code>server</code></td>
    <td>Enter the NFS file share server ID that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Enter the path to the NFS file share that you retrieved earlier.</td>
    </tr>
    </tbody></table>

3.  Create the PV in your cluster.

    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4.  Verify that the PV is created.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Create another configuration file to create your PVC. In order for the PVC to match the PV that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be an empty string. If any of these fields do not match the PV, then a new PV, and a new physical storage instance is [dynamically provisioned](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning).

    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "<size>"
     storageClassName: ""
    ```
    {: codeblock}

6.  Create your PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

7.  Verify that your PVC is created and bound to the PV. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Example output:

    ```
    Name: mypvc
    Namespace: default
    StorageClass:	""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


You successfully created a PV and bound it to a PVC. Cluster users can now [mount the PVC](#file_app_volume_mount) to their deployments and start reading from and writing to the PV object.

<br />


## Using {{site.data.keyword.filestorage_short}} in a stateful set
{: #file_statefulset}

If you have a stateful app such as a database, you can create stateful sets that use {{site.data.keyword.filestorage_short}} to store your app's data. Alternatively, you can use an {{site.data.keyword.cloud_notm}} database-as-a-service and store your data in the cloud.
{: shortdesc}

**What do I need to be aware of when adding {{site.data.keyword.filestorage_short}} to a stateful set?**

To add storage to a stateful set, you specify your storage configuration in the `volumeClaimTemplates` section of your stateful set YAML. The `volumeClaimTemplates` is the basis for your PVC and can include the storage class and the size or IOPS of your {{site.data.keyword.filestorage_short}} that you want to provision. However, if you want to include labels in your `volumeClaimTemplates`, Kubernetes does not include these labels when creating the PVC. Instead, you must add the labels directly to your stateful set.

You cannot deploy two stateful sets at the same time. If you try to create a stateful set before a different one is fully deployed, then the deployment of your stateful set might lead to unexpected results.
{: important}

**How can I create my stateful set in a specific zone?**

In a multizone cluster, you can specify the zone and region where you want to create your stateful set in the `spec.selector.matchLabels` and `spec.template.metadata.labels` section of your stateful set YAML. Alternatively, you can add those labels to a [customized storage class](/docs/containers?topic=containers-kube_concepts#customized_storageclass) and use this storage class in the `volumeClaimTemplates` section of your stateful set.

**Can I delay binding of a PV to my stateful pod until the pod is ready?**

Yes, you can [create a custom storage class](#file-topology) for your PVC that includes the [`volumeBindingMode: WaitForFirstConsumer`](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode){: external} field.

**What options do I have to add {{site.data.keyword.filestorage_short}} to a stateful set?**

If you want to automatically create your PVC when you create the stateful set, use [dynamic provisioning](#file_dynamic_statefulset). You can also choose to [pre-provision your PVCs or use existing PVCs](#file_static_statefulset) with your stateful set.  

### Dynamic provisioning: Creating the PVC when you create a stateful set
{: #file_dynamic_statefulset}

Use this option if you want to automatically create the PVC when you create the stateful set.
{: shortdesc}

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Verify that all existing stateful sets in your cluster are fully deployed. If a stateful set is still being deployed, you cannot start creating your stateful set. You must wait until all stateful sets in your cluster are fully deployed to avoid unexpected results.
   1. List existing stateful sets in your cluster.
      ```
      kubectl get statefulset --all-namespaces
      ```
      {: pre}

      Example output:
      ```
      NAME              DESIRED   CURRENT   AGE
      mystatefulset     3         3         6s
      ```
      {: screen}

   2. View the **Pods Status** of each stateful set to ensure that the deployment of the stateful set is finished.  
      ```
      kubectl describe statefulset <statefulset_name>
      ```
      {: pre}

      Example output:
      ```
      Name:               nginx
      Namespace:          default
      CreationTimestamp:  Fri, 05 Oct 2018 13:22:41 -0400
      Selector:           app=nginx,billingType=hourly,region=us-south,zone=dal10
      Labels:             app=nginx
                          billingType=hourly
                          region=us-south
                          zone=dal10
      Annotations:        kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"apps/v1","kind":"StatefulSet","metadata":{"annotations":{},"name":"nginx","namespace":"default"},"spec":{"podManagementPolicy":"Par...
      Replicas:           3 desired | 3 total
      Pods Status:        0 Running / 3 Waiting / 0 Succeeded / 0 Failed
      Pod Template:
        Labels:  app=nginx
                 billingType=hourly
                 region=us-south
                 zone=dal10
      ...
      ```
      {: screen}

      A stateful set is fully deployed when the number of replicas that you find in the **Replicas** section of your CLI output equals the number of **Running** pods in the **Pods Status** section. If a stateful set is not fully deployed yet, wait until the deployment is finished before you proceed.

2. Create a configuration file for your stateful set and the service that you use to expose the stateful set.

  - **Example stateful set that specifies a zone:**

    The following example shows how to deploy NGINX as a stateful set with 3 replicas. For each replica, a 20 gigabyte {{site.data.keyword.filestorage_short}} device is provisioned based on the specifications in the `ibmc-file-retain-bronze` storage class. All storage devices are provisioned in the `dal10` zone. Because {{site.data.keyword.filestorage_short}} cannot be accessed from other zones, all replicas of the stateful set are also deployed onto worker nodes that are located in `dal10`.

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
     name: nginx
    spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: Parallel
      selector:
        matchLabels:
          app: nginx
          billingType: "hourly"
          region: "us-south"
          zone: "dal10"
      template:
        metadata:
          labels:
            app: nginx
            billingType: "hourly"
            region: "us-south"
            zone: "dal10"
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol
              mountPath: /usr/share/nginx/html
       volumeClaimTemplates:
       - metadata:
           name: myvol
         spec:
           accessModes:
           - ReadWriteOnce
           resources:
             requests:
               storage: 20Gi
               iops: "300" #required only for performance storage
           storageClassName: ibmc-file-retain-bronze
    ```
    {: codeblock}

  - **Example stateful set with anti-affinity rule and delayed {{site.data.keyword.filestorage_short}} creation:**

    The following example shows how to deploy NGINX as a stateful set with 3 replicas. The stateful set does not specify the region and zone where the {{site.data.keyword.filestorage_short}} is created. Instead, the stateful set uses an anti-affinity rule to ensure that the pods are spread across worker nodes and zones. Worker node anti-affinity is achieved by defining the `app: nginx` label. This label instructs the Kubernetes scheduler to not schedule a pod on a worker node if a pod with the same label already runs on this worker node. The `topologykey: failure-domain.beta.kubernetes.io/zone` label restricts this anti-affinity rule even more and prevents the pod to be scheduled on a worker node that is located in the same zone as a worker node that already runs a pod with the `app: nginx` label. For each stateful set pod, two PVCs are created as defined in the `volumeClaimTemplates` section, but the creation of the {{site.data.keyword.filestorage_short}} instances is delayed until a stateful set pod that uses the storage is scheduled. This setup is referred to as [topology-aware volume scheduling](https://kubernetes.io/blog/2018/10/11/topology-aware-volume-provisioning-in-kubernetes/).

    ```yaml
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ibmc-file-bronze-delayed
    parameters:
      billingType: hourly
      classVersion: "2"
      iopsPerGB: "2"
      sizeRange: '[20-12000]Gi'
      type: Endurance
    provisioner: ibm.io/ibmc-file
    reclaimPolicy: Delete
    volumeBindingMode: WaitForFirstConsumer
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx
    ---
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 3
      podManagementPolicy: "Parallel"
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          affinity:
            podAntiAffinity:
              preferredDuringSchedulingIgnoredDuringExecution:
              - weight: 100
                podAffinityTerm:
                  labelSelector:
                    matchExpressions:
                    - key: app
                      operator: In
                      values:
                      - nginx
                  topologyKey: failure-domain.beta.kubernetes.io/zone
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: myvol1
              mountPath: /usr/share/nginx/html
            - name: myvol2
              mountPath: /tmp1
      volumeClaimTemplates:
      - metadata:
          name: myvol1
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
      - metadata:
          name: myvol2
        spec:
          accessModes:
          - ReadWriteMany # access mode
          resources:
            requests:
              storage: 20Gi
          storageClassName: ibmc-file-bronze-delayed
    ```
    {: codeblock}

    <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
    <caption>Understanding the stateful set YAML file components</caption>
    <thead>
      <th>Component</th>
      <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>name</code></td>
    <td style="text-align:left">In the metadata, enter a name for your stateful set. The name that you enter is used to create the name for your PVC in the format: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>serviceName</code></td>
    <td style="text-align:left">In the spec section, enter the name of the service that you want to use to expose your stateful set. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>replicas</code></td>
    <td style="text-align:left">Enter the number of replicas for your stateful set. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>podManagementPolicy</code></td>
    <td style="text-align:left">Enter the pod management policy that you want to use for your stateful set. Choose between the following options: <ul><li><strong><code>OrderedReady</code></strong>: With this option, stateful set replicas are deployed one after another. For example, if you specified 3 replicas, then Kubernetes creates the PVC for your first replica, waits until the PVC is bound, deploys the stateful set replica, and mounts the PVC to the replica. After the deployment is finished, the second replica is deployed. For more information about this option, see [`OrderedReady` Pod Management ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#orderedready-pod-management).</li><li><strong>Parallel: </strong>With this option, the deployment of all stateful set replicas is started at the same time. If your app supports parallel deployment of replicas, then use this option to save deployment time for your PVCs and stateful set replicas. </li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>matchLabels</code></td>
    <td style="text-align:left">In the spec selector section, enter all labels that you want to include in your stateful set and your PVC. Labels that you include in the <code>volumeClaimTemplates</code> of your stateful set are not recognized by Kubernetes. Sample labels that you might want to include are: <ul><li><code><strong>region</strong></code> and <code><strong>zone</strong></code>: If you want all your stateful set replicas and PVCs to be created in one specific zone, add both labels. You can also specify the zone and region in the storage class that you use. If you do not specify a zone and region and you have a multizone cluster, the zone in which your storage is provisioned is selected on a round-robin basis to balance volume requests evenly across all zones.</li><li><code><strong>billingType</strong></code>: Enter the billing type that you want to use for your PVCs. Choose between <code>hourly</code> or <code>monthly</code>. If you do not specify this label, all PVCs are created with an hourly billing type. </li></ul></td>
    </tr>
    <tr>
    <td style="text-align:left"><code>labels</code></td>
    <td style="text-align:left">In the spec template metadata section, enter the same labels that you added to the <code>spec.selector.matchLabels</code> section. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>affinity</code></td>
    <td style="text-align:left">In the spec template spec affinity section, specify your anti-affinity rule to ensure that your stateful set pods are distributed across worker nodes and zones. The example shows an anti-affinity rule where the stateful set pod prefers not to be scheduled on a worker node where a pod runs that has the `app: nginx` label. The `topologykey: failure-domain.beta.kubernetes.io/zone` restricts this anti-affinity rule even more and prevents the pod to be scheduled on a worker node if the worker node is in the same zone as a pod that has the `app: nginx` label. By using this anti-affinity rule, you can achieve anti-affinity across worker nodes and zones. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>name</code></td>
    <td style="text-align:left">In the spec volume claim templates metadata section, enter a name for your volume. Use the same name that you defined in the <code>spec.containers.volumeMount.name</code> section. The name that you enter here is used to create the name for your PVC in the format: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>storage</code></td>
    <td style="text-align:left">In the spec volume claim templates spec resources requests section, enter the size of the {{site.data.keyword.filestorage_short}} in gigabytes (Gi).</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>iops</code></td>
    <td style="text-align:left">In the spec volume claim templates spec resources requests section, if you want to provision [performance storage](#file_predefined_storageclass), enter the number of IOPS. If you use an endurance storage class and specify a number of IOPS, the number of IOPS is ignored. Instead, the IOPS that is specified in your storage class is used.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>storageClassName</code></td>
    <td style="text-align:left">In the spec volume claim templates spec section, enter the storage class that you want to use. To list existing storage classes, run <code>kubectl get storageclasses | grep file</code>. If you do not specify a storage class, the PVC is created with the default storage class that is set in your cluster. Make sure that the default storage class uses the <code>ibm.io/ibmc-file</code> provisioner so that your stateful set is provisioned with {{site.data.keyword.filestorage_short}}.</td>
    </tr>
    </tbody></table>

4. Create your stateful set.
   ```
   kubectl apply -f statefulset.yaml
   ```
   {: pre}

5. Wait for your stateful set to be deployed.
   ```
   kubectl describe statefulset <statefulset_name>
   ```
   {: pre}

   To see the current status of your PVCs, run `kubectl get pvc`. The name of your PVC is formatted as `<volume_name>-<statefulset_name>-<replica_number>`.
   {: tip}

### Static provisioning: Using an existing PVC with your stateful set
{: #file_static_statefulset}

You can pre-provision your PVCs before creating your stateful set or use existing PVCs with your stateful set.
{: shortdesc}

When you [dynamically provision your PVCs when creating the stateful set](#file_dynamic_statefulset), the name of the PVC is assigned based on the values that you used in the stateful set YAML file. In order for the stateful set to use existing PVCs, the name of your PVCs must match the name that would automatically be created when using dynamic provisioning.

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. If you want to pre-provision your PVC before you create the stateful set, follow steps 1-3 in [Adding {{site.data.keyword.filestorage_short}} to apps](#add_file) to create a PVC for each stateful set replica. Make sure that you create your PVC with a name that follows the following format: `<volume_name>-<statefulset_name>-<replica_number>`.
   - **`<volume_name>`**: Use the name that you want to specify in the `spec.volumeClaimTemplates.metadata.name` section of your stateful set, such as `nginxvol`.
   - **`<statefulset_name>`**: Use the name that you want to specify in the `metadata.name` section of your stateful set, such as `nginx_statefulset`.
   - **`<replica_number>`**: Enter the number of your replica starting with 0.

   For example, if you must create 3 stateful set replicas, create 3 PVCs with the following names: `nginxvol-nginx_statefulset-0`, `nginxvol-nginx_statefulset-1`, and `nginxvol-nginx_statefulset-2`.  

   Looking to create a PVC and PV for an existing {{site.data.keyword.filestorage_short}} instance? Create your PVC and PV by using [static provisioning](#existing_file).
   {: tip}

2. Follow the steps in [Dynamic provisioning: Creating the PVC when you create a stateful set](#file_dynamic_statefulset) to create your stateful set. The name of your PVC follows the format `<volume_name>-<statefulset_name>-<replica_number>`. Make sure to use the following values from your PVC name in the stateful set specification:
   - **`spec.volumeClaimTemplates.metadata.name`**: Enter the `<volume_name>` of your PVC name.
   - **`metadata.name`**: Enter the `<statefulset_name>` of your PVC name.
   - **`spec.replicas`**: Enter the number of replicas that you want to create for your stateful set. The number of replicas must equal the number of PVCs that you created earlier.

   If your PVCs are in different zones, do not include a region or zone label in your stateful set.
   {: note}

3. Verify that the PVCs are used in your stateful set replica pods.
   1. List the pods in your cluster. Identify the pods that belong to your stateful set.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Verify that your existing PVC is mounted to your stateful set replica. Review the **`ClaimName`** in the **`Volumes`** section of your CLI output.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

      Example output:
      ```
      Name:           nginx-0
      Namespace:      default
      Node:           10.xxx.xx.xxx/10.xxx.xx.xxx
      Start Time:     Fri, 05 Oct 2018 13:24:59 -0400
      ...
      Volumes:
        myvol:
          Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
          ClaimName:  myvol-nginx-0
     ...
      ```
      {: screen}

<br />

## Changing the size and IOPS of your existing storage device
{: #file_change_storage_configuration}

If you want to increase storage capacity or performance, you can modify your existing volume.
{: shortdesc}

For questions about billing and to find the steps for how to use the {{site.data.keyword.cloud_notm}} console to modify your storage, see [Expanding File Share capacity](/docs/FileStorage?topic=FileStorage-expandCapacity#expandCapacity).
{: tip}

1. List the PVCs in your cluster and note the name of the associated PV from the **VOLUME** column.
   ```
   kubectl get pvc
   ```
   {: pre}

   Example output:
   ```
   NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
   myvol            Bound     pvc-01ac123a-123b-12c3-abcd-0a1234cb12d3   20Gi       RWX            ibmc-file-bronze    147d
   ```
   {: screen}

2. Retrieve the **`StorageType`**, **`volumeId`**, and the **`server`** of the physical {{site.data.keyword.filestorage_short}} that is associated with your PVC by listing the details of the PV that your PVC is bound to. Replace `<pv_name>` with the name of the PV that you retrieved in the previous step. The storage type, volume ID, and the server name are shown in the **`Labels`** section of your CLI output.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Example output:
   ```
   Name:            pvc-4b62c704-5f77-11e8-8a75-b229c11ba64a
   Labels:          CapacityGb=20
                    Datacenter=dal10
                    Iops=2
                    StorageType=ENDURANCE
                    Username=IBM02SEV1543159_6
                    billingType=hourly
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal10
                    path=IBM01SEV1234567_8ab12t
                    server=fsf-dal1001g-fz.adn.networklayer.com
                    volumeId=12345678
   ...
   ```
   {: screen}

3. Modify the size or IOPS of your volume in your IBM Cloud infrastructure account.

   Example for performance storage:
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-iops <iops>
   ```
   {: pre}

   Example for endurance storage:
   ```
   ibmcloud sl file volume-modify <volume_ID> --new-size <size> --new-tier <iops>
   ```
   {: pre}

   <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
   <caption>Understanding the command components</caption>
   <thead>
      <th>Component</th>
      <th>Description</th>
    </thead>
   <tbody>
   <tr>
   <td><code>&lt;volume_ID&gt;</code></td>
   <td>Enter the ID of the volume that you retrieved earlier.</td>
   </tr>
   <tr>
   <td><code>&lt;new-size&gt;</code></td>
   <td>Enter the new size in gigabytes (Gi) for your volume. For valid sizes, see [Deciding on the {{site.data.keyword.filestorage_short}} configuration](#file_predefined_storageclass). The size that you enter must be greater than or equal to the current size of your volume. If you do not specify a new size, the current size of the volume is used. </td>
   </tr>
   <tr>
   <td><code>&lt;new-iops&gt;</code></td>
   <td>For performance storage only. Enter the new number of IOPS that you want. For valid IOPS, see [Deciding on the {{site.data.keyword.filestorage_short}} configuration](#file_predefined_storageclass). If you do not specify the IOPS, the current IOPS is used. <p class="note">If the original IOPS/GB ratio for the volume is less than 0.3, the new IOPS/GB ratio must be less than 0.3. If the original IOPS/GB ratio for the volume is greater than or equal to 0.3, the new IOPS/GB ratio for the volume must be greater than or equal to 0.3.</p> </td>
   </tr>
   <tr>
   <td><code>&lt;new-tier&gt;</code></td>
   <td>For endurance storage only. Enter the new number of IOPS per GB that you want. For valid IOPS, see [Deciding on the {{site.data.keyword.filestorage_short}} configuration](#file_predefined_storageclass). If you do not specify the IOPS, the current IOPS is used. <p class="note">If the original IOPS/GB ratio for the volume is less than 0.25, the new IOPS/GB ratio must be less than 0.25. If the original IOPS/GB ratio for the volume is greater than or equal to 0.25, the new IOPS/GB ratio for the volume must be greater than or equal to 0.25.</p> </td>
   </tr>
   </tbody>
   </table>

   Example output:
   ```
   Order 31020713 was placed successfully!.
   > Storage as a Service

   > 40 GBs

   > 2 IOPS per GB

   > 20 GB Storage Space (Snapshot Space)

   You may run 'ibmcloud sl file volume-list --order 12345667' to find this file volume after it is ready.
   ```
   {: screen}

4. If you changed the size of your volume and you use the volume in a pod, log in to your pod to verify the new size.
   1. List all the pods that use PVC.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Pods are returned in the format: `<pod_name>: <pvc_name>`.
   2. Log in to your pod.
      ```
      kubectl exec -it <pod_name> bash
      ```
      {: pre}

   3. Show the disk usage statistics and find the server path for your volume that you retrieved earlier.
      ```
      df -h
      ```
      {: pre}

      Example output:
      ```
      Filesystem                                                      Size  Used Avail Use% Mounted on
      overlay                                                          99G  4.8G   89G   6% /
      tmpfs                                                            64M     0   64M   0% /dev
      tmpfs                                                           7.9G     0  7.9G   0% /sys/fs/cgroup
      fsf-dal1001g-fz.adn.networklayer.com:/IBM01SEV1234567_6/data01   40G     0   40G   0% /myvol
      ```
      {: screen}

   Although the size and IOPS of your physical storage changed, these values are not reflected in your PV or PVC. If you describe your PV or PVC, the old size and IOPS continue to display. You have the option to manually update the size and IOPS in your PV by using the `kubectl patch pv` command. However, this command cannot be used to change the size or IOPS in the PVC. To prevent having different sizes and IOPS in your PVC and PV, leave both your PVC and PV as-is.
   {: important}

## Changing the default NFS version
{: #nfs_version}

The version of the {{site.data.keyword.filestorage_short}} determines the protocol that is used to communicate with the {{site.data.keyword.cloud_notm}} {{site.data.keyword.filestorage_short}} server. By default, all {{site.data.keyword.filestorage_short}} instances are set up with NFS version 4. You can change your existing PV to an older NFS version if your app requires a specific version to properly function.
{: shortdesc}

To change the default NFS version, you can either create a new storage class to dynamically provision {{site.data.keyword.filestorage_short}} in your cluster, or choose to change an existing PV that is mounted to your pod.

To apply the latest security updates and for a better performance, use the default NFS version and do not change to an older NFS version.
{: important}

**To create a customized storage class with a specific NFS version:**
1. Create a [customized storage class](#nfs_version_class) with the NFS version that you want to provision.
2. Create the storage class in your cluster.
   ```
   kubectl apply -f nfsversion_storageclass.yaml
   ```
   {: pre}

3. Verify that the customized storage class was created.
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Provision [{{site.data.keyword.filestorage_short}}](#add_file) with your customized storage class.

**To change your existing PV to use a different NFS version:**

1. Get the PV of the {{site.data.keyword.filestorage_short}} where you want to change the NFS version and note the name of the PV.
   ```
   kubectl get pv
   ```
   {: pre}

2. Add an annotation to your PV. Replace `<version_number>` with the NFS version that you want to use. For example, to change to NFS version 3.0, enter **3**.  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}

3. Delete the pod that uses the {{site.data.keyword.filestorage_short}} and re-create the pod.
   1. Save the pod YAML to your local machine.
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}

   2. Delete the pod.
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}

   3. Re-create the pod.
      ```
      kubectl apply -f pod.yaml
      ```
      {: pre}

4. Wait for the pod to deploy.
   ```
   kubectl get pods
   ```
   {: pre}

   The pod is fully deployed when the status changes to `Running`.

5. Log in to your pod.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verify that the {{site.data.keyword.filestorage_short}} was mounted with the NFS version that you specified earlier.
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}

   Example output:
   ```
   nfs vers=3.0
   ```
   {: screen}

<br />


## Scaling down the default {{site.data.keyword.filestorage_short}} plug-in
{: #file_scaledown_plugin}

By default, your classic {{site.data.keyword.containerlong_notm}} clusters include the {{site.data.keyword.filestorage_short}} plug-in. If you do not need to use {{site.data.keyword.filestorage_short}} in your cluster, you can conserve cluster resources by scaling down the plug-in and watcher components. Later, you can scale back up to one replica if you need {{site.data.keyword.filestorage_short}}. You cannot change other settings or remove the deployment entirely. Because the plug-in is still installed, it is updated along with the cluster version updates even if you scaled the plug-in down.
{: shortdesc}

Before you begin:
* [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Make sure that you have the **Manager** IAM service access role for the cluster, so that you can make changes to deployments in the `kube-system` namespace.

To scale down the {{site.data.keyword.filestorage_short}} plug-in:
1.  Scale down the {{site.data.keyword.filestorage_short}} plug-in and watcher deployments to `0` replicas.
    ```
    kubectl scale deployment -n kube-system --replicas=0 ibm-file-plugin
    ```
    {: pre}

    ```
    kubectl scale deployment -n kube-system --replicas=0 ibm-storage-watcher
    ```
    {: pre}

    If you need {{site.data.keyword.filestorage_short}} later, you can scale the plug-in back up with the following commands. `kubectl scale deployment -n kube-system --replicas=1 ibm-file-plugin && kubectl scale deployment -n kube-system --replicas=1 ibm-storage-watcher`
    {: tip}

2.  Optional: Confirm that the plug-in is scaled down. The scale-down is succesful when the pods are removed and remain removed even after the master state is changed, such as by a cluster refresh or update.

    1.  Confirm that the pods are removed.
        ```
        kubectl get pods -n kube-system -l 'app in (ibm-file-plugin, ibm-storage-watcher)'
        ```
        {: pre}

        Example output:
        ```
        No resources found.
        ```
        {: screen}
    2.  Refresh the cluster master.
        ```
        ibmcloud ks cluster refresh -c <cluster_name_or_ID>
        ```
        {: pre}
    3.  Wait a few minutes for the refresh to complete, then repeat substep `2.a` to check that the pods are removed. If the pods are rescheduled, the changes that you made to the {{site.data.keyword.filestorage_short}} plug-in configuration file were not properly saved. Make sure that your cluster runs the correct Kubernetes version, and try again.



## Backing up and restoring data
{: #file_backup_restore}

{{site.data.keyword.filestorage_short}} is provisioned into the same location as the worker nodes in your cluster. The storage is hosted on clustered servers by IBM to provide availability in case a server goes down. However, {{site.data.keyword.filestorage_short}} is not backed up automatically and might be inaccessible if the entire location fails. To protect your data from being lost or damaged, you can set up periodic backups that you can use to restore your data when needed.
{: shortdesc}

Review the following backup and restore options for your {{site.data.keyword.filestorage_short}}:

<dl>
  <dt>Set up periodic snapshots</dt>
  <dd><p>You can [set up periodic snapshots for your {{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-snapshots), which is a read-only image that captures the state of the instance at a point in time. To store the snapshot, you must request snapshot space on your {{site.data.keyword.filestorage_short}}. Snapshots are stored on the existing storage instance within the same zone. You can restore data from a snapshot if a user accidentally removes important data from the volume.</br> <strong>To create a snapshot for your volume: </strong><ol><li>[Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)</li><li>Log in to the `ibmcloud sl` CLI. <pre class="pre"><code>ibmcloud sl init</code></pre></li><li>List existing PVs in your cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Get the details for the PV for which you want to create snapshot space and note the volume ID, the size and the IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> The volume ID, the size and the IOPS can be found in the <strong>Labels</strong> section of your CLI output. </li><li>Create the snapshot size for your existing volume with the parameters that you retrieved in the previous step. <pre class="pre"><code>ibmcloud sl file snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Wait for the snapshot size to create. <pre class="pre"><code>ibmcloud sl file volume-detail &lt;volume_ID&gt;</code></pre>The snapshot size is successfully provisioned when the <strong>Snapshot Size (GB)</strong> in your CLI output changes from 0 to the size that you ordered. </li><li>Create the snapshot for your volume and note the ID of the snapshot that is created for you. <pre class="pre"><code>ibmcloud sl file snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Verify that the snapshot is created successfully. <pre class="pre"><code>ibmcloud sl file snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>To restore data from a snapshot to an existing volume: </strong><pre class="pre"><code>ibmcloud sl file snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Replicate snapshots to another zone</dt>
 <dd><p>To protect your data from a zone failure, you can [replicate snapshots](/docs/FileStorage?topic=FileStorage-replication#replication) to a {{site.data.keyword.filestorage_short}} instance that is set up in another zone. Data can be replicated from the primary storage to the backup storage only. You cannot mount a replicated {{site.data.keyword.filestorage_short}} instance to a cluster. When your primary storage fails, you can manually set your replicated backup storage to be the primary one. Then, you can mount it to your cluster. After your primary storage is restored, you can restore the data from the backup storage.</p></dd>
 <dt>Duplicate storage</dt>
 <dd><p>You can [duplicate your {{site.data.keyword.filestorage_short}} instance](/docs/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume) in the same zone as the original storage instance. A duplicate has the same data as the original storage instance at the point in time that you create the duplicate. Unlike replicas, use the duplicate as an independent storage instance from the original. To duplicate, first [set up snapshots for the volume](/docs/FileStorage?topic=FileStorage-snapshots).</p></dd>
  <dt>Back up data to {{site.data.keyword.cos_full}}</dt>
  <dd><p>You can use the [<code>ibm-backup-restore</code> Helm chart](/docs/containers?topic=containers-utilities#ibmcloud-backup-restore) to spin up a backup and restore pod in your cluster. This pod contains a script to run a one-time or periodic backup for any persistent volume claim (PVC) in your cluster. Data is stored in your {{site.data.keyword.cos_full}} instance that you set up in a zone.</p>
  <p>To make your data even more highly available and protect your app from a zone failure, set up a second {{site.data.keyword.cos_full}} instance and replicate data across zones. If you need to restore data from your {{site.data.keyword.cos_full}} instance, use the restore script that is provided with the Helm chart.</p></dd>
<dt>Copy data to and from pods and containers</dt>
<dd><p>You can use the `kubectl cp` [command![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) to copy files and directories to and from pods or specific containers in your cluster.</p>
<p>Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) If you do not specify a container with <code>-c</code>, the command uses the first available container in the pod.</p>
<p>You can use the command in various ways:</p>
<ul>
<li>Copy data from your local machine to a pod in your cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copy data from a pod in your cluster to your local machine: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copy data from your local machine to a specific container that runs in a pod in your cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>

<br />

## Storage class reference
{: #file_storageclass_reference}

Storage classes that have `retain` in the title have a reclaim policy of **Retain**. Example: `ibmc-file-retain-bronze`. Storage classes that do not have `retain` in the title have a reclaim policy of **Delete**. Example: `ibmc-file-bronze`.
{: tip}

| Characteristics | Setting|
|:-----------------|:-----------------|
| Name | <code>ibmc-file-bronze</code></br><code>ibmc-file-retain-bronze</code></br><code>ibmc-file-bronze-gid</code> |
| Type | Endurance storage|
| File system | NFS|
| IOPS per gigabyte | 2|
| Size range in gigabytes | 20-12000 Gi|
| Hard disk | SSD|
| Reclaim policy | <code>ibmc-file-bronze</code>: Delete</br><code>ibmc-file-retain-bronze</code>: Retain </br><code>ibmc-file-bronze-gid:</code> Delete|
| Supplemental group ID | The supplemental group ID 65531 is automatically set when you use the <code>ibmc-file-bronze-gid</code> storage class to allow non-root users access to your file storage instance. For more information about how to use this storage class or set custom group IDs, see [File storage: Adding non-root user access to persistent storage fails](/docs/containers?topic=containers-cs_troubleshoot_storage#cs_storage_nonroot).  |
| Billing | Hourly|
| Pricing | [Pricing information](https://www.ibm.com/cloud/file-storage/pricing){: external}|
{: class="simple-tab-table"}
{: caption="{{site.data.keyword.filestorage_short}} class: bronze" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Bronze"}
{: tab-group="{{site.data.keyword.filestorage_short}} class"}

| Characteristics | Setting|
|:-----------------|:-----------------|
| Name | <code>ibmc-file-silver</code></br><code>ibmc-file-retain-silver</code></br><code>ibmc-file-silver-gid</code> |
| Type | Endurance storage|
| File system | NFS|
| IOPS per gigabyte | 4|
| Size range in gigabytes | 20-12000 Gi|
| Hard disk | SSD|
| Reclaim policy | <code>ibmc-file-silver</code>: Delete</br><code>ibmc-file-retain-silver</code>: Retain </br><code>ibmc-file-silver-gid:</code> Delete |
| Supplemental group ID | The supplemental group ID 65531 is automatically set when you use the <code>ibmc-file-bronze-gid</code> storage class to allow non-root users access to your file storage instance. For more information about how to use this storage class or set custom group IDs, see [File storage: Adding non-root user access to persistent storage fails](/docs/containers?topic=containers-cs_troubleshoot_storage#cs_storage_nonroot).  |
| Billing | Hourly|
| Pricing | [Pricing information](https://www.ibm.com/cloud/file-storage/pricing){: external}|
{: class="simple-tab-table"}
{: caption="{{site.data.keyword.filestorage_short}} class: silver" caption-side="top"}
{: #simpletabtable2}
{: tab-title="Silver"}
{: tab-group="{{site.data.keyword.filestorage_short}} class"}

| Characteristics | Setting|
|:-----------------|:-----------------|
| Name | <code>ibmc-file-gold</code></br><code>ibmc-file-retain-gold</code></br><code>ibmc-file-gold-gid</code>  |
| Type | Endurance storage|
| File system | NFS|
| IOPS per gigabyte | 10|
| Size range in gigabytes | 20-4000 Gi|
| Hard disk | SSD|
| Reclaim policy | <code>ibmc-file-gold</code>: Delete</br><code>ibmc-file-retain-gold</code>: Retain </br><code>ibmc-file-gold-gid:</code> Delete |
| Supplemental group ID | The supplemental group ID 65531 is automatically set when you use the <code>ibmc-file-bronze-gid</code> storage class to allow non-root users access to your file storage instance. For more information about how to use this storage class or set custom group IDs, see [File storage: Adding non-root user access to persistent storage fails](/docs/containers?topic=containers-cs_troubleshoot_storage#cs_storage_nonroot).  |
| Billing | Hourly|
| Pricing | [Pricing information](https://www.ibm.com/cloud/file-storage/pricing){: external}|
{: class="simple-tab-table"}
{: caption="{{site.data.keyword.filestorage_short}} class: gold" caption-side="top"}
{: #simpletabtable3}
{: tab-title="Gold"}
{: tab-group="{{site.data.keyword.filestorage_short}} class"}

| Characteristics | Setting|
|:-----------------|:-----------------|
| Name | <code>ibmc-file-custom</code></br><code>ibmc-file-retain-custom</code> |
| Type | Performance|
| File system | NFS|
| IOPS and size | <p><strong>Size range in gigabytes / IOPS range in multiples of 100</strong></p><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul>|
| Hard disk | The IOPS to gigabyte ratio determines the type of hard disk that is provisioned. To determine your IOPS to gigabyte ratio, you divide the IOPS by the size of your storage. </br></br>Example: </br>You chose 500Gi of storage with 100 IOPS. Your ratio is 0.2 (100 IOPS/500Gi). </br></br><strong>Overview of hard disk types per ratio:</strong><ul><li>Less than or equal to 0.3: SATA</li><li>Greater than 0.3: SSD</li></ul>|
| Reclaim policy | <code>ibmc-file-custom</code>: Delete</br><code>ibmc-file-retain-custom</code>: Retain |
| Billing | Hourly|
| Pricing | [Pricing information](https://www.ibm.com/cloud/file-storage/pricing){: external}|
{: class="simple-tab-table"}
{: caption="{{site.data.keyword.filestorage_short}} class: custom" caption-side="top"}
{: #simpletabtable4}
{: tab-title="Custom"}
{: tab-group="{{site.data.keyword.filestorage_short}} class"}

<br />

## Sample customized storage classes
{: #file_custom_storageclass}

You can create a customized storage class and use the storage class in your PVC.
{: shortdesc}

{{site.data.keyword.containerlong_notm}} provides [pre-defined storage classes](#file_storageclass_reference) to provision {{site.data.keyword.filestorage_short}} with a particular tier and configuration. In some cases, you might want to provision storage with a different configuration that is not covered in the pre-defined storage classes. You can use the examples in this topic to find sample customized storage classes.

To create your customized storage class, see [Customizing a storage class](/docs/containers?topic=containers-kube_concepts#customized_storageclass). Then, [use your customized storage class in your PVC](#add_file).

### Creating topology-aware storage
{: #file-topology}

To use {{site.data.keyword.filestorage_short}} in a multizone cluster, your pod must be scheduled in the same zone as your {{site.data.keyword.filestorage_short}} instance so that you can read and write to the volume. Before topology-aware volume scheduling was introduced by Kubernetes, the dynamic provisioning of your storage automatically created the {{site.data.keyword.filestorage_short}} instance when a PVC was created. Then, when you created your pod, the Kubernetes scheduler tried to deploy the pod to a worker node in the same data center as your {{site.data.keyword.filestorage_short}} instance.
{: shortdesc}

Creating the {{site.data.keyword.filestorage_short}} instance without knowing the constraints of the pod can lead to unwanted results. For example, your pod might not be able to be scheduled to the same worker node as your storage because the worker node has insufficient resources or the worker node is tainted and does not allow the pod to be scheduled. With topology-aware volume scheduling, the {{site.data.keyword.filestorage_short}} instance is delayed until the first pod that uses the storage is created.

Topology-aware volume scheduling is supported on clusters that run Kubernetes version 1.12 or later only.
{: note}

The following examples show how to create storage classes that delay the creation of the {{site.data.keyword.filestorage_short}} instance until the first pod that uses this storage is ready to be scheduled. To delay the creation, you must include the `volumeBindingMode: WaitForFirstConsumer` option. If you do not include this option, the `volumeBindingMode` is automatically set to `Immediate` and the {{site.data.keyword.filestorage_short}} instance is created when you create the PVC.

- **Example for Endurance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-bronze-delayed
  parameters:
    billingType: hourly
    classVersion: "2"
    iopsPerGB: "2"
    sizeRange: '[20-12000]Gi'
    type: Endurance
  provisioner: ibm.io/ibmc-file
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

- **Example for Performance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: WaitForFirstConsumer
  ```
  {: codeblock}

### Specifying the zone for multizone clusters
{: #file_multizone_yaml}

If you want to create your {{site.data.keyword.filestorage_short}} in a specific zone, you can specify the zone and region in a customized storage class.
{: shortdesc}

Use the customized storage class if you want to [statically provision {{site.data.keyword.filestorage_short}}](#existing_file) in a specific zone. In all other cases, [specify the zone directly in your PVC](#add_file).
{: note}

When you create the customized storage class, specify the same region and zone that your cluster and worker nodes are in. To get the region of your cluster, run `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` and look for the region prefix in the **Master URL**, such as `eu-de` in `https://c2.eu-de.containers.cloud.ibm.com:11111`. To get the zone of your worker node, run `ibmcloud ks worker ls --cluster <cluster_name_or_ID>`.

- **Example for Endurance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-silver-mycustom-storageclass
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    zone: "dal12"
    region: "us-south"
    type: "Endurance"
    iopsPerGB: "4"
    sizeRange: "[20-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

- **Example for Performance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
   name: ibmc-file-performance-storageclass
   labels:
     kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   zone: "dal12"
   region: "us-south"
   billingType: "hourly"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   type: "Performance"
  reclaimPolicy: Delete
  volumeBindingMode: Immediate
  ```
  {: codeblock}

### Changing the default NFS version
{: #nfs_version_class}

The following customized storage class lets you define the NFS version that you want to provision. For example, to provision NFS version 3.0, replace `<nfs_version>` with **3.0**.
{: shortdesc}

- **Example for Endurance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
    type: "Endurance"
    iopsPerGB: "2"
    sizeRange: "[1-12000]Gi"
    reclaimPolicy: "Delete"
    classVersion: "2"
    mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}

- **Example for Performance {{site.data.keyword.filestorage_short}}:**
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: ibmc-file-mount
    labels:
      kubernetes.io/cluster-service: "true"
  provisioner: ibm.io/ibmc-file
  parameters:
   type: "Performance"
   classVersion: "2"
   sizeIOPSRange: |-
     "[20-39]Gi:[100-1000]"
     "[40-79]Gi:[100-2000]"
     "[80-99]Gi:[100-4000]"
     "[100-499]Gi:[100-6000]"
     "[500-999]Gi:[100-10000]"
     "[1000-1999]Gi:[100-20000]"
     "[2000-2999]Gi:[200-40000]"
     "[3000-3999]Gi:[200-48000]"
     "[4000-7999]Gi:[300-48000]"
     "[8000-9999]Gi:[500-48000]"
     "[10000-12000]Gi:[1000-48000]"
   mountOptions: nfsvers=<nfs_version>
  ```
  {: codeblock}

## Removing persistent storage from a cluster
{: #cleanup}

When you set up persistent storage in your cluster, you have three main components: the Kubernetes persistent volume claim (PVC) that requests storage, the Kubernetes persistent volume (PV) that is mounted to a pod and described in the PVC, and the IBM Cloud infrastructure instance, such as classic file or block storage. Depending on how you created your storage, you might need to delete all three components separately. 
{: shortdesc}

### Understanding your storage removal options
{: #storage_delete_options}

Removing persistent storage from your {{site.data.keyword.cloud_notm}} account varies depending on how you provisioned the storage and what components you already removed.
{: shortdesc}

**Is my persistent storage deleted when I delete my cluster?**

During cluster deletion, you have the option to remove your persistent storage. However, depending on how your storage was provisioned, the removal of your storage might not include all storage components.

If you [dynamically provisioned](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning) storage with a storage class that sets `reclaimPolicy: Delete`, your PVC, PV, and the storage instance are automatically deleted when you delete the cluster. For storage that was [statically provisioned](/docs/containers?topic=containers-kube_concepts#static_provisioning), VPC Block Storage, or storage that you provisioned with a storage class that sets `reclaimPolicy: Retain`, the PVC and the PV are removed when you delete the cluster, but your storage instance and your data remain. You are still charged for your storage instance. Also, if you deleted your cluster in an unhealthy state, the storage might still exist even if you chose to remove it.

**How do I delete the storage when I want to keep my cluster?**

When you dynamically provisioned the storage with a storage class that sets `reclaimPolicy: Delete`, you can remove the PVC to start the deletion process of your persistent storage. Your PVC, PV, and storage instance are automatically removed.

For storage that was [statically provisioned](/docs/containers?topic=containers-kube_concepts#static_provisioning), VPC Block Storage, or storage that you provisioned with a storage class that sets `reclaimPolicy: Retain`, you must manually remove the PVC, PV, and the storage instance to avoid further charges.

**How does the billing stop after I delete my storage?**

Depending on what storage components you delete and when, the billing cycle might not stop immediately. If you delete the PVC and PV, but not the storage instance in your {{site.data.keyword.cloud_notm}} account, that instance still exists and you are charged for it.

If you delete the PVC, PV, and the storage instance, the billing cycle stops depending on the `billingType` that you chose when you provisioned your storage and how you chose to delete the storage.

- When you manually cancel the persistent storage instance from the {{site.data.keyword.cloud_notm}} console or the `ibmcloud sl` CLI, billing stops as follows:
  - **Hourly storage**: Billing stops immediately. After your storage is canceled, you might still see your storage instance in the console for up to 72 hours.
  - **Monthly storage**: You can choose between immediate cancellation or cancellation on the anniversary date. In both cases, you are billed until the end of the current billing cycle, and billing stops for the next billing cycle. After your storage is canceled, you might still see your storage instance in the console or the CLI for up to 72 hours.
    - **Immediate cancellation**: Choose this option to immediately remove your storage. Neither you nor your users can use the storage anymore or recover the data.
    - **Anniversary date**: Choose this option to cancel your storage on the next anniversary date. Your storage instances remain active until the next anniversary date and you can continue to use them until this date, such as to give your team time to make backups of your data.

- When you dynamically provisioned the storage with a storage class that sets `reclaimPolicy: Delete` and you choose to remove the PVC, the PV and the storage instance are immediately removed. For hourly billed storage, billing stops immediately. For monthly billed storage, you are still charged for the remainder of the month. After your storage is removed and billing stops, you might still see your storage instance in the console or the CLI for up to 72 hours.

**What do I need to be aware of before I delete persistent storage?**

When you clean up persistent storage, you delete all the data that is stored in it. If you need a copy of the data, make a backup for [file storage](/docs/containers?topic=containers-file_storage#file_backup_restore) or [block storage](/docs/containers?topic=containers-block_storage#block_backup_restore).

**I deleted my storage instance. Why can I still see my instance?**

After you remove persistent storage, it can take up to 72 hours for the removal to be fully processed and for the storage to disappear from your {{site.data.keyword.cloud_notm}} console or CLI.


### Cleaning up persistent storage
{: #storage_remove}

Remove the PVC, PV, and the storage instance from your {{site.data.keyword.cloud_notm}} account to avoid further charges for your persistent storage.
{: shortdesc}

Before you begin:
- Make sure that you backed up any data that you want to keep.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To clean up persistent data:

1.  List the PVCs in your cluster and note the **`NAME`** of the PVC, the **`STORAGECLASS`**, and the name of the PV that is bound to the PVC and shown as **`VOLUME`**.
    ```
    kubectl get pvc
    ```
    {: pre}

    Example output:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}

2. Review the **`ReclaimPolicy`** and **`billingType`** for the storage class.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   If the reclaim policy says `Delete`, your PV and the physical storage are removed when you remove the PVC. Note that VPC Block Storage is not removed automatically, even if you used a `Delete` storage class to provision the storage. If the reclaim policy says `Retain`, or if you provisioned your storage without a storage class, then your PV and physical storage are not removed when you remove the PVC. You must remove the PVC, PV, and the physical storage separately.

   If your storage is charged monthly, you still get charged for the entire month, even if you remove the storage before the end of the billing cycle.
   {: important}

3. Remove any pods that mount the PVC.
   1. List the pods that mount the PVC.
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Example output:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}

      If no pod is returned in your CLI output, you do not have a pod that uses the PVC.

   2. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment.
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}

   3. Verify that the pod is removed.
      ```
      kubectl get pods
      ```
      {: pre}

4. Remove the PVC.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. Review the status of your PV. Use the name of the PV that you retrieved earlier as **`VOLUME`**.
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}

   When you remove the PVC, the PV that is bound to the PVC is released. Depending on how you provisioned your storage, your PV goes into a `Deleting` state if the PV is deleted automatically, or into a `Released` state, if you must manually delete the PV. **Note**: For PVs that are automatically deleted, the status might briefly say `Released` before it is deleted. Rerun the command after a few minutes to see whether the PV is removed.

6. If your PV is not deleted, manually remove the PV.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

7. Verify that the PV is removed.
   ```
   kubectl get pv
   ```
   {: pre}
8. {: #sl_delete_storage}List the physical storage instance that your PV pointed to and note the **`id`** of the physical storage instance.
    ```
    ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
    ```
    {: pre}
    Example output for {{site.data.keyword.filestorage_short}}:
    ```
    id         notes   
    12345678   {"plugin":"ibm-file-plugin-5b55b7b77b-55bb7","region":"us-south","cluster":"aa1a11a1a11b2b2bb22b22222c3c3333","type":"Endurance","ns":"default","pvc":"mypvc","pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7","storageclass":"ibmc-file-gold"}
    ```
    {: screen}
    Understanding the **Notes** field information:
    *  **`"plugin":"ibm-file-plugin-5b55b7b77b-55bb7"`**: The storage plug-in that the cluster uses.
    *  **`"region":"us-south"`**: The region that your cluster is in.
    *  **`"cluster":"aa1a11a1a11b2b2bb22b22222c3c3333"`**: The cluster ID that is associated with the storage instance.
    *  **`"type":"Endurance"`**: The type of file or block storage, either `Endurance` or `Performance`.
    *  **`"ns":"default"`**: The namespace that the storage instance is deployed to.
    *  **`"pvc":"mypvc"`**: The name of the PVC that is associated with the storage instance.
    *  **`"pv":"pvc-d979977d-d79d-77d9-9d7d-d7d97ddd99d7"`**: The PV that is associated with the storage instance.
    *  **`"storageclass":"ibmc-file-gold"`**: The type of storage class: bronze, silver, gold, or custom.

9. Remove the physical storage instance.

    ```
    ibmcloud sl file volume-cancel <classic_file_id>
    ```
    {: pre}

10. Verify that the physical storage instance is removed.

    The deletion process might take up to 72 hours to complete.
    {: important}

    ```
    ibmcloud sl file volume-list
    ```
    {: pre}


