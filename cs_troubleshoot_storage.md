---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, help, debug

subcollection: containers
content-type: troubleshoot

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
 


# Persistent storage
{: #cs_troubleshoot_storage}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting persistent storage.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Debugging persistent storage failures
{: #debug_storage}
{: troubleshoot}
{: support}

Review the options to debug persistent storage and find the root causes for failures.
{: shortdesc}

1. Check whether the pod that mounts your storage instance is successfully deployed.
   1. List the pods in your cluster. A pod is successfully deployed if the pod shows a status of **Running**.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Get the details of your pod and check whether errors are displayed in the **Events** section of your CLI output.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}


   3. Retrieve the logs for your app and check whether you can see any error messages.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}

2. Restart your pod. If your pod is part of a deployment, delete the pod and let the deployment rebuild it. If your pod is not part of a deployment, delete the pod and reapply your pod configuration file.
  ```
  kubectl delete pod <pod_name>
  ```
  {: pre}

  ```
  kubectl apply -f <app.yaml>
  ```
  {: pre}

3. If restarting your pod does not resolve the issue, [reload your worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).

4. Verify that you use the latest {{site.data.keyword.cloud_notm}} and {{site.data.keyword.containerlong_notm}} plug-in version.
   ```
   ibmcloud update
   ```
   {: pre}

   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

5. Verify that the storage driver and plug-in pods show a status of **Running**.
   1. List the pods in the `kube-system` namespace.
      ```
      kubectl get pods -n kube-system
      ```
      {: pre}

   2. If the storage driver and plug-in pods do not show a **Running** status, get more details of the pod to find the root cause. Depending on the status of your pod, you might not be able to execute all of the following commands.
      1. Get the names of the containers that run in the driver pod.
         ```
         kubectl get pod <pod_name> -n kube-system -o jsonpath="{.spec['containers','initContainers'][*].name}" | tr -s '[[:space:]]' '\n'
         ```
         {: pre}

         Example output for {{site.data.keyword.block_storage_is_short}} with three containers:
         ```
         csi-provisioner
         csi-attacher
         iks-vpc-block-driver
         ```
         {: screen}

         Example output for {{site.data.keyword.blockstorageshort}}:
         ```
         ibmcloud-block-storage-driver-container
         ```
         {: pre}

      2. Export the logs from the driver pod to a `logs.txt` file on your local machine. Include the driver container name.

         ```
         kubectl logs <pod_name> -n kube-system -c <container_name> > logs.txt
         ```
         {: pre}

      3. Review the log file.
         ```
         cat logs.txt
         ```
         {: pre}

   3. Analyze the **Events** section of the CLI output of the `kubectl describe pod` command and the latest logs to find the root cause for the error.

6. Check whether your PVC is successfully provisioned.
   1. Check the state of your PVC. A PVC is successfully provisioned if the PVC shows a status of **Bound**.
      ```
      kubectl get pvc
      ```
      {: pre}

   2. If the state of the PVC shows **Pending**, retrieve the error for why the PVC remains pending.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}

   3. Review common errors that can occur during the PVC creation.
      - [File storage and classic block storage: PVC remains in a pending state](#file_pvc_pending)
      - [Object storage: PVC remains in a pending state](#cos_pvc_pending)

   4. Review common errors that can occur when you mount a PVC to your app.
      - [File storage: App cannot access or write to PVC](#file_app_failures)
      - [Classic Block storage: App cannot access or write to PVC](#block_app_failures)
      - [Object storage: Accessing files with a non-root user fails](#cos_nonroot_access)


7. Verify that the `kubectl` CLI version that you run on your local machine matches the Kubernetes version that is installed in your cluster. If you use a `kubectl` CLI version that does not match at least the major.minor version of your cluster, you might experience unexpected results. For example, [Kubernetes does not support ![External link icon](../icons/launch-glyph.svg “External link icon”)](https://kubernetes.io/docs/setup/release/version-skew-policy/) `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2).
   1. Show the `kubectl` CLI version that is installed in your cluster and your local machine.
      ```
      kubectl version
      ```
      {: pre}

      Example output:
      ```
      Client Version: version.Info{Major:"1", Minor:"1.19", GitVersion:"v1.19.8", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"1.19", GitVersion:"v1.19.8+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}

      The CLI versions match if you can see the same version in `GitVersion` for the client and the server. You can ignore the `+IKS` part of the version for the server.
   2. If the `kubectl` CLI versions on your local machine and your cluster do not match, either [update your cluster](/docs/containers?topic=containers-update) or [install a different CLI version on your local machine](/docs/containers?topic=containers-cs_cli_install#kubectl).


8. For {{site.data.keyword.block_storage_is_short}}, [verify that you have the latest version of the add-on](/docs/containers?topic=containers-vpc-block#vpc-addon-update).


9. For classic block storage, object storage, and Portworx only: Make sure that you installed the latest Helm chart version for the plug-in.

   **Block and object storage**:

   1. Update your Helm chart repositories.
      ```
      helm repo update
      ```
      {: pre}

   2. List the Helm charts in the repository.
      **For classic block storage**:
        ```
        helm search repo iks-charts | grep block-storage-plugin
        ```
        {: pre}

        Example output:
        ```
        iks-charts-stage/ibmcloud-block-storage-plugin	1.5.0        	                                        	A Helm chart for installing ibmcloud block storage plugin   
        iks-charts/ibmcloud-block-storage-plugin      	1.5.0        	                                        	A Helm chart for installing ibmcloud block storage plugin   
        ```
        {: screen}

      **For object storage**:
        ```
        helm search repo ibm-charts | grep object-storage-plugin
        ```
        {: pre}

        Example output:
        ```
        ibm-charts/ibm-object-storage-plugin         	1.0.9        	1.0.9                         	A Helm chart for installing ibmcloud object storage plugin  
        ```
        {: screen}

   3. List the installed Helm charts in your cluster and compare the version that you installed with the version that is available.
      ```
      helm list --all-namespaces
      ```
      {: pre}

   4. If a more recent version is available, install this version. For instructions, see [Updating the {{site.data.keyword.cloud_notm}} Block Storage plug-in](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) and [Updating the {{site.data.keyword.cos_full_notm}} plug-in](/docs/containers?topic=containers-object_storage#update_cos_plugin).

   **Portworx**:

   1. Find the [latest Helm chart version](https://github.com/IBM/charts/tree/master/community/portworx){: external} that is available.

   2. List the installed Helm charts in your cluster and compare the version that you installed with the version that is available.
      ```
      helm list --all-namespaces
      ```
      {: pre}

   3. If a more recent version is available, install this version. For instructions, see [Updating Portworx in your cluster](/docs/containers?topic=containers-portworx#update_portworx).



## File storage and block storage: PVC remains in a pending state
{: #file_pvc_pending}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you create a PVC and you run `kubectl get pvc <pvc_name>`, your PVC remains in a **Pending** state, even after waiting for some time.

{: tsCauses}
During the PVC creation and binding, many different tasks are executed by the file and block storage plug-in. Each task can fail and cause a different error message.

{: tsResolve}

1. Find the root cause for why the PVC remains in a **Pending** state.
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}

2. Review common error message descriptions and resolutions.

   <table summary="The columns are read from left to right. The first column has the error message. The second column describes the error message. The third column provides steps to resolve the error.">
   <thead>
     <th>Error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name &lt;datacenter_name&gt;.</code></td>
      <td>The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster does not have all the required permissions to provision persistent storage. </td>
      <td>See [PVC creation fails because of missing permissions](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>Every {{site.data.keyword.cloud_notm}} account is set up with a maximum number of file and block storage instances that can be created. By creating the PVC, you exceed the maximum number of storage instances. For more information about the maximum number of volumes that you can create and how to retrieve the number of volumes in your account, see the documentation for [file](/docs/FileStorage?topic=FileStorage-managinglimits) and [block](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits) storage.</td>
      <td>To create a PVC, choose from the following options. <ul><li>Remove any unused PVCs.</li><li>Ask the {{site.data.keyword.cloud_notm}} account owner to increase your storage quota by [opening a support case](/docs/get-support?topic=get-support-using-avatar).</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>The storage size and IOPS that you specified in your PVC are not supported by the storage type that you chose and cannot be used with the specified storage class. </td>
      <td>Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class that you want to use. Correct the size and IOPS, and re-create the PVC. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>The data center that you specified in your PVC does not exist. </td>
  <td>Run <code>ibmcloud ks locations</code> to list available data centers. Correct the data center in your PVC and re-create the PVC.</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>The storage size, IOPS, and storage type might be incompatible with the storage class that you chose, or the {{site.data.keyword.cloud_notm}} infrastructure API endpoint is currently unavailable. </td>
  <td>Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class and storage type that you want to use. Then, delete the PVC and re-create the PVC. </td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>You want to create a PVC for an existing storage instance by using Kubernetes static provisioning, but the storage instance that you specified could not be found. </td>
  <td>Follow the instructions to provision existing [file storage](/docs/containers?topic=containers-file_storage#existing_file) or [block storage](/docs/containers?topic=containers-block_storage#existing_block) in your cluster and make sure to retrieve the correct information for your existing storage instance. Then, delete the PVC and re-create the PVC.  </td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>You created a custom storage class and specified a storage type that is not supported.</td>
  <td>Update your custom storage class to specify `Endurance` or `Performance` as your storage type. To find examples for custom storage classes, see the sample custom storage classes for [file storage](/docs/containers?topic=containers-file_storage#file_custom_storageclass) and [block storage](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>

## File storage: App cannot access or write to PVC
{: #file_app_failures}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

When you mount a PVC to your pod, you might experience errors when accessing or writing to the PVC.
{: shortdesc}

1. List the pods in your cluster and review the status of the pod.
   ```
   kubectl get pods
   ```
   {: pre}

2. Find the root cause for why your app cannot access or write to the PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}

   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Review common errors that can occur when you mount a PVC to a pod.
   <table summary="The columns are read from left to right. The first column has the symptom or error message. The second column describes the message. The third column provides steps to resolve the error.">
   <thead>
     <th>Symptom or error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td>Your pod is stuck in a <strong>ContainerCreating</strong> state. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>The {{site.data.keyword.cloud_notm}} infrastructure back end experienced network problems. To protect your data and to avoid data corruption, {{site.data.keyword.cloud_notm}} automatically disconnected the file storage server to prevent write operations on NFS file shares.  </td>
      <td>See [File storage: File systems for worker nodes change to read-only](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>In your deployment, you specified a non-root user to own the NFS file storage mount path. By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. </td>
  <td>See [File storage: App fails when a non-root user owns the NFS file storage mount path](#nonroot)</td>
  </tr>
  <tr>
  <td>After you specified a non-root user to own the NFS file storage mount path or deployed a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.</td>
  <td>The deployment or Helm chart configuration specifies the security context for the pod's <code>fsGroup</code> (group ID) and <code>runAsUser</code> (user ID)</td>
  <td>See [File storage: Adding non-root user access to persistent storage fails](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### File storage: File systems for worker nodes change to read-only
{: #readonly_nodes}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

{: tsSymptoms}
{: #stuck_creating_state}
You might see one of the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods that are running on the same worker node are stuck in the `ContainerCreating` state.
- When you run a `kubectl describe` command, you see the following error in the **Events** section: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
The file system on the worker node is read-only.

{: tsResolve}
1.  Back up any data that might be stored on the worker node or in your containers.
2.  For a short-term fix to the existing worker node, reload the worker node.
    <pre class="pre"><code>ibmcloud ks worker reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

For a long-term fix, [update the flavor of your worker pool](/docs/containers?topic=containers-update#machine_type).

<br />


### File storage: App fails when a non-root user owns the NFS file storage mount path
{: #nonroot}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

{: tsSymptoms}
After you [add NFS storage](/docs/containers?topic=containers-file_storage#file_app_volume_mount) to your deployment, the deployment of your container fails. When you retrieve the logs for your container, you might see errors such as the following. The pod fails and is stuck in a reload cycle.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. Some common app images, such as Jenkins and Nexus3, specify a non-root user that owns the mount path in the Dockerfile. When you create a container from this Dockerfile, the creation of the container fails due to insufficient permissions of the non-root user on the mount path. To grant write permission, you can modify the Dockerfile to temporarily add the non-root user to the root user group before it changes the mount path permissions, or use an init container.

If you use a Helm chart to deploy the image, edit the Helm deployment to use an init container.
{:tip}



{: tsResolve}
When you include an [init container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/){: external} in your deployment, you can give a non-root user that is specified in your Dockerfile write permissions for the volume mount path inside the container. The init container starts before your app container starts. The init container creates the volume mount path inside the container, changes the mount path to be owned by the correct (non-root) user, and closes. Then, your app container starts with the non-root user that must write to the mount path. Because the path is already owned by the non-root user, writing to the mount path is successful. If you do not want to use an init container, you can modify the Dockerfile to add non-root user access to NFS file storage.


**Before you begin**: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Open the Dockerfile for your app and get the user ID (UID) and group ID (GID) from the user that you want to give writer permission on the volume mount path. In the example from a Jenkins Dockerfile, the information is:
    - UID: `1000`
    - GID: `1000`

    **Example**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update && apt-get install -y git curl && rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Add persistent storage to your app by creating a persistent volume claim (PVC). This example uses the `ibmc-file-bronze` storage class. To review available storage classes, run `kubectl get storageclasses`.

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Create the PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  In your deployment `.yaml` file, add the init container. Include the UID and GID that you previously retrieved.

    ```yaml
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Example** for a Jenkins deployment:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Create the pod and mount the PVC to your pod.

    ```
    kubectl apply -f my-pod.yaml
    ```
    {: pre}

6. Verify that the volume is successfully mounted to your pod. Note the pod name and **Containers/Mounts** path.
   ```
   kubectl describe pod <my-pod>
   ```
   {: pre}

   **Example output**:
   ```
   Name:       mypod-123456789
   Namespace:	default
   ...
   Init Containers:
   ...
   Mounts:
     /mount from volume (rw)
     /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
   ...
   Containers:
     jenkins:
       Container ID:
       Image:		jenkins
       Image ID:
       Port:		  <none>
       State:		Waiting
         Reason:		PodInitializing
       Ready:		False
       Restart Count:	0
       Environment:	<none>
       Mounts:
         /var/jenkins_home from volume (rw)
         /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
   ...
   Volumes:
     myvol:
       Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
       ClaimName:	mypvc
       ReadOnly:	  false

   ```
   {: screen}

7.  Log in to the pod by using the pod name that you previously noted.
    ```
    kubectl exec -it <my-pod-123456789> /bin/bash
    ```
    {: pre}

8. Verify the permissions of your container's mount path. In the example, the mount path is `/var/jenkins_home`.
   ```
   ls -ln /var/jenkins_home
   ```
   {: pre}

   **Example output**:
   ```
   jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
   total 12
   -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
   drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
   drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
   ```
   {: screen}

   This output shows that the GID and UID from your Dockerfile (in this example, `1000` and `1000`) own the mount path inside the container.

<br />

### File storage: Adding non-root user access to persistent storage fails
{: #cs_storage_nonroot}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

{: tsSymptoms}
After you [add non-root user access to persistent storage](#nonroot) or deploy a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.

{: tsCauses}
Your app deployment or Helm chart configuration specifies the [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for the pod's `fsGroup` (group ID) and `runAsUser` (user ID). Generally, a pod's default security context sets [`runAsNonRoot`](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) so that the pod cannot run as the root user. Because the `fsGroup` setting is not designed for shared storage such as NFS file storage, the `fsGroup` setting is not supported, and the `runAsUser` setting is automatically set to `2020`. These default settings do not allow other non-root users to write to the mounted storage.

{: tsResolve}
To allow a non-root user read and write access to a file storage device, you must allocate a [supplemental group ID](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups){: external} in a storage class, refer to this storage class in the PVC, and set the pod's security context with a `runAsUser` value that is automatically added to the supplemental group ID. When you grant the supplemental group ID read and write access to the file storage, any non-root user that belongs to the group ID, including your pod, is granted access to the file storage.

You can use one of the provided [`gid` storage classes](/docs/containers?topic=containers-file_storage#file_storageclass_reference) or create a custom storage class to define your own supplemental group ID.

Allocating a supplemental group ID for a non-root user of a file storage device is supported for single zone clusters only, and cannot be used in multizone clusters.
{: note}

1. Select one of the [provided `gid` storage classes](/docs/containers?topic=containers-file_storage#file_storageclass_reference) to assign the default group ID `65531` to your non-root user that you want to read and write to your file storage. If you want to assign a custom group ID, create a YAML file for a customized storage class. In your customized storage class YAML file, include the `gidAllocate: "true"` parameter and define the group ID in the `gidFixed` parameter.

   **Example storage classes for assigning the default group ID `65531`**:
   - `ibmc-file-bronze-gid`
   - `ibmc-file-silver-gid`
   - `ibmc-file-gold-gid`

   **Example customized storage class to specify a different group ID**:
   ```yaml
   apiVersion: storage.k8s.io/v1beta1
   kind: StorageClass
   metadata:
     name: ibmc-file-bronze-gid-custom
     labels:
       kubernetes.io/cluster-service: "true"
   provisioner: ibm.io/ibmc-file
   parameters:
     type: "Endurance"
     iopsPerGB: "2"
     sizeRange: "[1-12000]Gi"
     mountOptions: nfsvers=4.1,hard
     billingType: "hourly"
     reclaimPolicy: "Delete"
     classVersion: "2"
     gidAllocate: "true"
     gidFixed: "65165"
   ```
   {: codeblock}

   To create the storage class in your cluster, run `kubectl apply -f storageclass.yaml`.

3. Create a YAML file for your PVC that uses the storage class that you created.

   ```yaml
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
   name: gid-pvc
   labels:
     billingType: "monthly"
   spec:
   accessModes:
     - ReadWriteMany
   resources:
     requests:
       storage: 20Gi
   storageClassName: ibmc-file-bronze-gid
   ```
   {: codeblock}

4. Create the PVC in your cluster.
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}

5. Wait a few minutes for the file storage to be provisioned and the PVC to change to a `Bound` status.
   ```
   kubectl get pvc
   ```
   {: pre}

   If you tried creating the PVC in a multizone cluster, the PVC remains in a `pending` state.
   {: note}

   Example output:
   ```
   NAME      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS           AGE
   gid-pvc   Bound    pvc-5e4acab4-9b6f-4278-b53c-22e1d3ffa123   20Gi       RWX            ibmc-file-bronze-gid   2m54s
   ```
   {: screen}

6. Create a YAML file for your deployment that mounts the PVC that you created. In the `spec.template.spec.securityContext.runAsUser` field, specify the non-root user ID that you want to use. This user ID is automatically added to the supplemental group ID that is defined in the storage class to gain read and write access to the file storage.

   **Example for creating an `node-hello` deployment**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: gid-deployment
     labels:
       app: gid
   spec:
     selector:
       matchLabels:
         app: gid
     template:
       metadata:
         labels:
           app: gid
       spec:
         containers:
         - image: gcr.io/google-samples/node-hello:1.0
           name: gid-container
           volumeMounts:
           - name: gid-vol
             mountPath: /myvol
         securityContext:
           runAsUser: 2020
         volumes:
         - name: gid-vol
           persistentVolumeClaim:
             claimName: gid-pvc
   ```
   {: codeblock}

7. Create the deployment in your cluster.
   ```
   kubectl apply -f deployment.yaml
   ```
   {: pre}

8. Verify that your pod is in a **Running** status.
   ```
   kubectl get pods
   ```
   {: pre}

   Example output:
   ```
   NAME                              READY   STATUS    RESTARTS   AGE
   gid-deployment-5dc86db4c4-5hbts   2/2     Running   0          69s
   ```
   {: screen}

9. Log in to your pod.
    ```
    kubectl exec <pod_name> -it bash
    ```
    {: pre}

10. Verify the read and write permissions for the non-root user.
    1. List the user ID and group IDs for the current user inside the pod.
       ```
       id
       ```
       {: pre}

       Example output:
       ```
       uid=2020 gid=0(root) groups=0(root), 65531
       ```
       {: screen}

       The setup is correct if your non-root user ID is listed as `uid` and the supplemental group ID that you defined in your storage class is listed under `groups`.

    2. List the permissions for your volume mount directory that you defined in your deployment.
       ```
       ls -l /<volume_mount_path>
       ```
       {: pre}

       Example output:
       ```
       drwxrwxr-x 2 nobody 65531 4096 Dec 11 07:40 .
       drwxr-xr-x 1 root   root  4096 Dec 11 07:30 ..
       ```
       {: screen}

       The setup is correct if the supplemental group ID that you defined in your storage class is listed with read and write permissions in your volume mount directory.

   3. Create a file in your mount directory.
      ```
      echo "Able to write to file storage with my non-root user." > /myvol/gidtest.txt
      ```
      {: pre}

   4. List the permissions for the files in your volume mount directory.
      ```
      ls -al /mnt/nfsvol/
      ```
      {: pre}

      Example output:
      ```
      drwxrwxr-x 2 nobody      65531 4096 Dec 11 07:40 .
      drwxr-xr-x 1 root        root  4096 Dec 11 07:30 ..
      -rw-r--r-- 1 2020   4294967294   42 Dec 11 07:40 gidtest.txt .
      ```
      {: screen}

      In your CLI output, the non-root user ID is listed with read and write access to the file that you created.

   5. Exit your pod.
      ```
      exit
      ```
      {: pre}


If you need to change the ownership of the mount path from `nobody`, see [App fails when a non-root user owns the NFS file storage mount path](#nonroot).
{: tip}

<br />



## Block Storage: App cannot access or write to PVC
{: #block_app_failures}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

When you mount a PVC to your pod, you might experience errors when accessing or writing to the PVC.
{: shortdesc}

1. List the pods in your cluster and review the status of the pod.
   ```
   kubectl get pods
   ```
   {: pre}

2. Find the root cause for why your app cannot access or write to the PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}

   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Review common errors that can occur when you mount a PVC to a pod.
   <table summary="The columns are read from left to right. The first column has the symptom or error message. The second column describes the message. The third column provides steps to resolve the error.">
   <thead>
     <th>Symptom or error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td>Your pod is stuck in a <strong>ContainerCreating</strong> or <strong>CrashLoopBackOff</strong> state. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>The {{site.data.keyword.cloud_notm}} infrastructure back end experienced network problems. To protect your data and to avoid data corruption, {{site.data.keyword.cloud_notm}} automatically disconnected the block storage server to prevent write operations on block storage instances.  </td>
      <td>See [Block storage: Block storage changes to read-only](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>You want to mount an existing block storage instance that was set up with an <code>XFS</code> file system. When you created the PV and the matching PVC, you specified an <code>ext4</code> or no file system. The file system that you specify in your PV must be the same file system that is set up in your block storage instance. </td>
  <td>See [Block storage: Mounting existing block storage to a pod fails due to the wrong file system](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Block storage: Block storage changes to read-only
{: #readonly_block}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
You might see the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods on the same worker node are stuck in the `ContainerCreating` or `CrashLoopBackOff` state. All these pods use the same block storage instance.
- When you run a `kubectl describe pod` command, you see the following error in the **Events** section: `MountVolume.SetUp failed for volume ... read-only`.

{: tsCauses}
If a network error occurs while a pod writes to a volume, IBM Cloud infrastructure protects the data on the volume from getting corrupted by changing the volume to a read-only mode. Pods that use this volume cannot continue to write to the volume and fail.

{: tsResolve}
1. Check the version of the {{site.data.keyword.cloud_notm}} Block Storage plug-in that is installed in your cluster.
   ```
   helm list --all-namespaces
   ```
   {: pre}

2. Verify that you use the [latest version of the {{site.data.keyword.cloud_notm}} Block Storage plug-in](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-block-storage-plugin). If not, [update your plug-in](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. If you used a Kubernetes deployment for your pod, restart the pod that is failing by removing the pod and letting Kubernetes re-create it. If you did not use a deployment, retrieve the YAML file that was used to create your pod by running `kubectl get pod <pod_name> -o yaml >pod.yaml`. Then, delete and manually re-create the pod.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Check whether re-creating your pod resolved the issue. If not, reload the worker node.
   1. Find the worker node where your pod runs and note the private IP address that is assigned to your worker node.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Example output:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Retrieve the **ID** of your worker node by using the private IP address from the previous step.
      ```
      ibmcloud ks worker ls --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. Gracefully [reload the worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).


<br />

### Block storage: Mounting existing block storage to a pod fails due to the wrong file system
{: #block_filesystem}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you run `kubectl describe pod <pod_name>`, you see the following error:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
You have an existing block storage device that is set up with an `XFS` file system. To mount this device to your pod, you [created a PV](/docs/containers?topic=containers-block_storage#existing_block) that specified `ext4` as your file system or no file system in the `spec/flexVolume/fsType` section. If no file system is defined, the PV defaults to `ext4`.
The PV was created successfully and was linked to your existing block storage instance. However, when you try to mount the PV to your cluster by using a matching PVC, the volume fails to mount. You cannot mount your `XFS` block storage instance with an `ext4` file system to the pod.

{: tsResolve}
Update the file system in the existing PV from `ext4` to `XFS`.

1. List the existing PVs in your cluster and note the name of the PV that you used for your existing block storage instance.
   ```
   kubectl get pv
   ```
   {: pre}

2. Save the PV YAML on your local machine.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Open the YAML file and change the `fsType` from `ext4` to `xfs`.
4. Replace the PV in your cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Log in to the pod where you mounted the PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verify that the file system changed to `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Example output:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />

## Block storage: Installing the Block storage plug-in Helm chart gives CPU throttling warnings
{: #block_helm_cpu}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

{: tsSymptoms}
When you install the Block storage Helm chart, the installation gives a warning similar to the following:

```
Message: 50% throttling of CPU in namespace kube-system for container ibmcloud-block-storage-driver-container in pod ibmcloud-block-storage-driver-1abab.
```
{: screen}

{: tsCauses}
The default Block storage plug-in resource requests are not sufficient. The Block storage plug-in and driver are installed with the following default resource request and limit values.

```yaml
plugin:
  resources:
    requests:
      memory: 100Mi
      cpu: 50m
    limits:
      memory: 300Mi
      cpu: 300m
driver:
  resources:
    requests:
      memory: 50Mi
      cpu: 25m
    limits:
      memory: 200Mi
      cpu: 100m
```
{: codeblock}

{: tsResolve}
Remove and reinstall the Helm chart with increased resource requests and limits.

1. Remove the Helm chart.
   ```
   helm uninstall <release_name> iks-charts/ibmcloud-block-storage-plugin -n <namespace>
   ```
   {: pre}

2. Reinstall the Helm chart and increase the resource requests and limits by using the `--set` flag when running `helm install`. The following example command sets the `plugin.resources.requests.memory` value to `200Mi` and the `plugin.resources.requests.cpu` value to `100m`. You can pass multiple values by using the `--set` flag for each value that you want to pass.

   ```
   helm install <release_name> iks-charts/ibmcloud-block-storage-plugin -n <namespace> --set plugin.resources.requests.memory=200Mi --set plugin.resources.requests.cpu=100m
   ```
   {: pre}

## Object storage: Installing the Object storage `ibmc` Helm plug-in fails
{: #cos_helm_fails}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you install the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in, the installation fails with one of the following errors:
```
Error: symlink /Users/iks-charts/ibm-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
When the `ibmc` Helm plug-in is installed, a symlink is created from the `~/.helm/plugins/helm-ibmc` directory for Linux systems or the `~/Library/helm/plugins/helm-ibmc` directory for Mac OS to the directory where the `ibmc` Helm plug-in is located on your local system, which is usually in `./ibmcloud-object-storage-plugin/helm-ibmc`. When you remove the `ibmc` Helm plug-in from your local system, or you move the `ibmc` Helm plug-in directory to a different location, the symlink is not removed.

If you see a `permission denied` error, you do not have the required `read`, `write`, and `execute` permission on the `ibmc.sh` bash file so that you can execute `ibmc` Helm plug-in commands.

{: tsResolve}

**For symlink errors**:

1. Remove the {{site.data.keyword.cos_full_notm}} Helm plug-in.
   **Linux**
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

   **Mac OS**
   ```
   rm -rf ~/Library/helm/plugins/helm-ibmc for macOS
   ```
   {: pre}

2. Follow the [documentation](/docs/containers?topic=containers-object_storage#install_cos) to re-install the `ibmc` Helm plug-in and the {{site.data.keyword.cos_full_notm}} plug-in.

**For permission errors**:

1. Change the permissions for the `ibmc` plug-in.
   **Linux**
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}

   **Mac OS**
   ```
   chmod 755 ~/Library/helm/plugins/helm-ibmc/ibmc.sh
   ```

2. Try out the `ibm` Helm plug-in.
   ```
   helm ibmc --help
   ```
   {: pre}

3. [Continue installing the {{site.data.keyword.cos_full_notm}} plug-in](/docs/containers?topic=containers-object_storage#install_cos).


## Object storage: Installing the Object storage plug-in fails
{: #cos_plugin_fails}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you install the `ibm-object-storage-plugin`, the installation fails with an error similar to the following:
```
Error: rendered manifest contains a resource that already exists. Unable to continue with install. Existing resource conflict: namespace: , name: ibmc-s3fs-flex-cross-region, existing_kind: storageClass, new_kind: storage.k8s.io/v1, Kind=StorageClass
Error: plugin "ibmc" exited with error
```
{: screen}

{: tsCauses}
During the installation, many different tasks are executed by the {{site.data.keyword.cos_full_notm}} plug-in such as creating storage classes and cluster role bindings. Some of these resources might already exist in your cluster from previous {{site.data.keyword.cos_full_notm}} plug-in installations and were not properly removed when you removed or upgraded the plug-in.

{: tsResolve}
1. Delete the resource that is displayed in the error message.
   ```
   kubectl delete <resource_kind> <resource_name>
   ```
   {: pre}

   Example for deleting a storage class resource:
   ```
   kubectl delete storageclass <storage_class_name>
   ```
   {: pre}

2. [Retry the installation](/docs/containers?topic=containers-object_storage#install_cos).

3. If you continue to see the same error, get a list of the resources that are installed when the plug-in is installed.

   1. Get a list of storage classes that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get StorageClass --all-namespaces \
          -l app=ibmcloud-object-storage-plugin \
          -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   2. Get a list of cluster role bindings that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get ClusterRoleBinding --all-namespaces \
          -l app=ibmcloud-object-storage-plugin \
          -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   3. Get a list of role bindings that are created by the `ibmcloud-object-storage-driver`.
      ```
      kubectl get RoleBinding --all-namespaces \
         -l app=ibmcloud-object-storage-driver \
         -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   4. Get a list of role bindings that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get RoleBinding --all-namespaces \
          -l app=ibmcloud-object-storage-plugin \
          -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   5. Get a list of cluster roles that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get ClusterRole --all-namespaces \
         -l app=ibmcloud-object-storage-plugin \
         -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   6. Get a list of deployments that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get Deployments --all-namespaces \
         -l app=ibmcloud-object-storage-plugin \
         -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   7. Get a list of the daemon sets that are created by the `ibmcloud-object-storage-driver`.
      ```
      kubectl get DaemonSets --all-namespaces \
         -l app=ibmcloud-object-storage-driver \
         -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

   8. Get a list of the service accounts that are created by the `ibmcloud-object-storage-driver`.
      ```
      kubectl get ServiceAccount --all-namespaces \
         -l app=ibmcloud-object-storage-driver \
         -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre }

   9. Get a list of the service accounts that are created by the `ibmcloud-object-storage-plugin`.
      ```
      kubectl get ServiceAccount --all-namespaces \
         -l app=ibmcloud-object-storage-plugin \
         -o jsonpath='{range .items[*]}{.metadata.namespace}{"\t"}{.metadata.name}{"\n"}{end}'
      ```
      {: pre}

4. Delete the conflicting resources.

5. After you delete the conflicting resources, [retry the installation](/docs/containers?topic=containers-object_storage#install_cos).



<br />

## Object storage: PVC remains in a pending state
{: #cos_pvc_pending}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create a PVC and you run `kubectl get pvc <pvc_name>`, your PVC remains in a **Pending** state, even after waiting for some time.

{: tsCauses}
During the PVC creation and binding, many different tasks are executed by the {{site.data.keyword.cos_full_notm}} plug-in. Each task can fail and cause a different error message.

{: tsResolve}

1. Find the root cause for why the PVC remains in a **Pending** state.
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}

2. Review common error message descriptions and resolutions.

   <table summary="The columns are read from left to right. The first column has the symptom or error message. The second column describes the message. The third column provides steps to resolve the error.">
   <thead>
   <th>Error message</th>
   <th>Description</th>
   <th>Steps to resolve</th>
   </thead>
   <tbody>
   <tr>
   <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
   <td>The Kubernetes secret that holds your {{site.data.keyword.cos_full_notm}} service credentials does not exist in the same namespace as the PVC or pod. </td>
   <td>See [PVC or pod creation fails due to not finding the Kubernetes secret](#cos_secret_access_fails).</td>
   </tr>
   <tr>
   <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
   <td>The Kubernetes secret that you created for your {{site.data.keyword.cos_full_notm}} service instance does not include the `type: ibm/ibmc-s3fs`.</td>
   <td>Edit the Kubernetes secret that holds your {{site.data.keyword.cos_full_notm}} credentials to add or change the `type` to `ibm/ibmc-s3fs`. </td>
   </tr>
   <tr>
   <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> <code>Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname></code></td>
   <td>The s3fs API or IAM API endpoint has the wrong format, or the s3fs API endpoint could not be retrieved based on your cluster location.  </td>
   <td>See [PVC creation fails due to wrong s3fs API endpoint](#cos_api_endpoint_failure).</td>
   </tr>
   <tr>
   <td>`object-path cannot be set when auto-create is enabled`</td>
   <td>You specified an existing subdirectory in your bucket that you want to mount to your PVC by using the `ibm.io/object-path` annotation. If you set a subdirectory, you must disable the bucket auto-create feature.  </td>
   <td>In your PVC, set `ibm.io/auto-create-bucket: "false"` and provide the name of the existing bucket in `ibm.io/bucket`.</td>
   </tr>
   <tr>
   <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
   <td>In your PVC, you set `ibm.io/auto-delete-bucket: true` to automatically delete your data, the bucket, and the PV when you remove the PVC. This option requires `ibm.io/auto-create-bucket` to be set to <strong>true</strong>, and `ibm.io/bucket` to be set to `""` at the same time.</td>
   <td>In your PVC, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""` so that your bucket is automatically created with a name with the format `tmp-s3fs-xxxx`. </td>
   </tr>
   <tr>
   <td>`bucket cannot be set when auto-delete is enabled`</td>
   <td>In your PVC, you set `ibm.io/auto-delete-bucket: true` to automatically delete your data, the bucket, and the PV when you remove the PVC. This option requires `ibm.io/auto-create-bucket` to be set to <strong>true</strong>, and `ibm.io/bucket` to be set to `""` at the same time.</td>
   <td>In your PVC, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""` so that your bucket is automatically created with a name with the format `tmp-s3fs-xxxx`. </td>
   </tr>
   <tr>
   <td>`cannot create bucket using API key without service-instance-id`</td>
   <td>If you want to use IAM API keys to access your {{site.data.keyword.cos_full_notm}} service instance, you must store the API key and the ID of the {{site.data.keyword.cos_full_notm}} service instance in a Kubernetes secret.  </td>
   <td>See [Creating a secret for the object storage service credentials](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
   </tr>
   <tr>
   <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
   <td>You specified an existing subdirectory in your bucket that you want to mount to your PVC by using the `ibm.io/object-path` annotation. This subdirectory could not be found in the bucket that you specified. </td>
   <td>Verify that the subdirectory that you specified in `ibm.io/object-path` exists in the bucket that you specified in `ibm.io/bucket`. </td>
   </tr>
   <tr>
   <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
   <td>You set `ibm.io/auto-create-bucket: true` and specified a bucket name at the same time, or you specified a bucket name that already exists in {{site.data.keyword.cos_full_notm}}. Bucket names must be unique across all service instances and regions in {{site.data.keyword.cos_full_notm}}. </td>
   <td>Make sure that you set `ibm.io/auto-create-bucket: false` and that you provide a bucket name that is unique in {{site.data.keyword.cos_full_notm}}. If you want to use the {{site.data.keyword.cos_full_notm}} plug-in to automatically create a bucket name for you, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""`. Your bucket is created with a unique name in the format `tmp-s3fs-xxxx`. If you want to specify a bucket name for the auto-created bucket, set `ibm.io/auto-create-bucket: true` and `ibm.io/auto-delete-bucket: false` and `ibm.io/bucket: "<bucket_name>"`.</td>
   </tr>
   <tr>
   <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
   <td>You tried to access a bucket that you did not create, or the storage class and s3fs API endpoint that you specified do not match the storage class and s3fs API endpoint that were used when the bucket was created. </td>
   <td>See [Cannot access an existing bucket](#cos_access_bucket_fails). </td>
   </tr>
   <tr>
   <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
   <td>The values in your Kubernetes secret are not correctly encoded to base64.  </td>
   <td>Review the values in your Kubernetes secret and encode every value to base64. You can also use the [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) command to create a new secret and let Kubernetes automatically encode your values to base64. </td>
   </tr>
   <tr>
   <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
   <td>You specified `ibm.io/auto-create-bucket: false` and tried to access a bucket that you did not create or the secret access key or access key ID of your {{site.data.keyword.cos_full_notm}} HMAC credentials are incorrect.  </td>
   <td>You cannot access a bucket that you did not create. Automatically create a bucket instead by setting `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""`. If you are the owner of the bucket, see [PVC creation fails due to wrong credentials or access denied](#cred_failure) to check your credentials. </td>
   </tr>
   <tr>
   <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
   <td>You specified `ibm.io/auto-create-bucket: true` to automatically create a bucket in {{site.data.keyword.cos_full_notm}}, but the credentials that you provided in the Kubernetes secret are assigned the **Reader** IAM service access role. This role does not allow bucket creation in {{site.data.keyword.cos_full_notm}}. </td>
   <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
   </tr>
   <tr>
   <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
   <td>You specified `ibm.io/auto-create-bucket: true` and provided a name of an existing bucket in `ibm.io/bucket`. In addition the credentials that you provided in the Kubernetes secret are assigned the **Reader** IAM service access role. This role does not allow bucket creation in {{site.data.keyword.cos_full_notm}}. </td>
   <td>To use an existing bucket, set `ibm.io/auto-create-bucket: false` and provide the name of your existing bucket in `ibm.io/bucket`. To automatically create a bucket by using your existing Kubernetes secret, set `ibm.io/bucket: ""` and follow [PVC creation fails due to wrong credentials or access denied](#cred_failure) to verify the credentials in your Kubernetes secret.</td>
   </tr>
   <tr>
   <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
   <td>The {{site.data.keyword.cos_full_notm}} secret access key of your HMAC credentials that you provided in your Kubernetes secret is not correct. </td>
   <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure).</td>
   </tr>
   <tr>
   <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
   <td>The {{site.data.keyword.cos_full_notm}} access key ID or the secret access key of your HMAC credentials that you provided in your Kubernetes secret is not correct.</td>
   <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
   </tr>
   <tr>
   <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
   <td>The {{site.data.keyword.cos_full_notm}} API key of your IAM credentials and the GUID of your {{site.data.keyword.cos_full_notm}} service instance are not correct.</td>
   <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
   </tr>
   <tr>
   <td><code>TokenManagerRetrieveError: error retrieving the token</code></td>
   <td>This error occurs when you create a PVC with IAM credentials on a cluster that does not have public outbound access.</td>
   <td>If your cluster does not have public outbound access, [create a {{site.data.keyword.cos_full_notm}} instance that uses HMAC credentials](/docs/containers?topic=containers-object_storage#create_cos_service).</td>
   </tr>
   <tr>
   <td><code>set-access-policy not supported for classic cluster</code></td>
   <td>This error occurs when you install the <code>ibm-object-storage-plugin</code> in a Classic cluster and set the <code>bucketAccessPolicy=true</code> flag. The <code>bucketAccessPolicy=true</code> flag is only used with VPC Gen 2 clusters.</td>
   <td>[Install the plug-in](/docs/containers?topic=containers-object_storage#install_cos) and set the <code>bucketAccessPolicy=false</code> flag.</td>
   </tr>
   </tbody>
   </table>


### Object storage: PVC or pod creation fails due to not finding the Kubernetes secret
{: #cos_secret_access_fails}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create your PVC or deploy a pod that mounts the PVC, the creation or deployment fails.

- Example error message for a PVC creation failure:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Example error message for a pod creation failure:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
The Kubernetes secret that you created is not referenced correctly in your deployment yaml or is not set to the `ibm/ibmc-s3fs` type.

{: tsResolve}
This task requires [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role](/docs/containers?topic=containers-users#platform) for all namespaces.

1. List the secrets in your cluster and review the secret type. The secret must show `ibm/ibmc-s3fs` as the **Type**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. If your secret does not show `ibm/ibmc-s3fs` as the **Type**, [re-create your secret](/docs/containers?topic=containers-object_storage#create_cos_secret).

3. Check your YAML configuration file for your PVC and pod to verify that you used the correct secret.


### Object storage: PVC creation fails due to wrong credentials or access denied
{: #cred_failure}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create the PVC, you see an error message similar to one of the following:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
The {{site.data.keyword.cos_full_notm}} service credentials that you use to access the service instance might be wrong, or allow only read access to your bucket.

{: tsResolve}
1. In the navigation on the service details page, click **Service Credentials**.
2. Find your credentials, then click **View credentials**.
3. In the **iam_role_crn** section, verify that you have the `Writer` or `Manager` role. If you do not have the correct role, you must create new {{site.data.keyword.cos_full_notm}} service credentials with the correct permission.
4. If the role is correct, verify that you use the correct **access_key_id** and **secret_access_key** in your Kubernetes secret.
5. [Create a new secret with the updated **access_key_id** and **secret_access_key**](/docs/containers?topic=containers-object_storage#create_cos_secret).


<br />

### Object storage: PVC creation fails due to wrong s3fs or IAM API endpoint
{: #cos_api_endpoint_failure}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create the PVC, the PVC remains in a pending state. After you run the `kubectl describe pvc <pvc_name>` command, you see one of the following error messages:

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
The s3fs API endpoint for the bucket that you want to use might have the wrong format, or your cluster is deployed in a location that is supported in {{site.data.keyword.containerlong_notm}} but is not yet supported by the {{site.data.keyword.cos_full_notm}} plug-in.

{: tsResolve}
1. Check the s3fs API endpoint that was automatically assigned by the `ibmc` Helm plug-in to your storage classes during the {{site.data.keyword.cos_full_notm}} plug-in installation. The endpoint is based on the location that your cluster is deployed to.  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   If the command returns `ibm.io/object-store-endpoint: NA`, your cluster is deployed in a location that is supported in {{site.data.keyword.containerlong_notm}} but is not yet supported by the {{site.data.keyword.cos_full_notm}} plug-in. To add the location to the {{site.data.keyword.containerlong_notm}}, post a question in our public Slack or open an {{site.data.keyword.cloud_notm}} support case. For more information, see [Getting help and support](#getting_help_storage).

2. If you manually added the s3fs API endpoint with the `ibm.io/endpoint` annotation or the IAM API endpoint with the `ibm.io/iam-endpoint` annotation in your PVC, make sure that you added the endpoints in the format `https://<s3fs_api_endpoint>` and `https://<iam_api_endpoint>`. The annotation overwrites the API endpoints that are automatically set by the `ibmc` plug-in in the {{site.data.keyword.cos_full_notm}} storage classes.
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

<br />

### Object storage: Cannot access an existing bucket
{: #cos_access_bucket_fails}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create the PVC, the bucket in {{site.data.keyword.cos_full_notm}} cannot be accessed. You see an error message similar to the following:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
You might have used the wrong storage class to access your existing bucket, or you tried to access a bucket that you did not create. You cannot access a bucket that you did not create.

{: tsResolve}
1. From the [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com/){: external}, select your {{site.data.keyword.cos_full_notm}} service instance.
2. Select **Buckets**.
3. Review the **Class** and **Location** information for your existing bucket.
4. Choose the appropriate [storage class](/docs/containers?topic=containers-object_storage#cos_storageclass_reference).
5. Make sure that you provide the correct name of your existing bucket.

<br />

## Object storage: Changing the ownership of the mount path fails
{: #cos_mountpath_error}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
You created a deployment that mounts an {{site.data.keyword.cos_full_notm}} bucket to your app. During the deployment, you try to change the ownership of the volume mount path, but this action fails. You see error messages similar to the following:

```
chown: changing ownership of '<volume_mount_path>': Input/output error
chown: changing ownership of '<volume_mount_path>': Operation not permitted
```
{: screen}

{: tsCauses}
When you create a bucket in {{site.data.keyword.cos_full_notm}}, the bucket is managed by `s3fs-fuse`. The UID and GID that own the volume mount path are automatically set by Fuse when you mount the bucket to your app and cannot be changed.

{: tsResolve}
You cannot change the ownership of the volume mount path. However, you can change the UID and GID for a file or a directory that is stored under your volume mount path. For more information, see [Object storage: Accessing files with a non-root user fails](#cos_nonroot_access).


## Object storage: Accessing files with a non-root user fails
{: #cos_nonroot_access}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
You uploaded files to your {{site.data.keyword.cos_full_notm}} service instance by using the console or the REST API. When you try to access these files with a non-root user that you defined with `runAsUser` in your app deployment, access to the files is denied.

{: tsCauses}
In Linux, a file or a directory has three access groups: `Owner`, `Group`, and `Other`. When you upload a file to {{site.data.keyword.cos_full_notm}} by using the console or the REST API, the permissions for the `Owner`, `Group`, and `Other` are removed. The permission of each file looks as follows:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

When you upload a file by using the {{site.data.keyword.cos_full_notm}} plug-in, the permissions for the file are preserved and not changed.

{: tsResolve}
To access the file with a non-root user, the non-root user must have read and write permissions for the file. Changing the permission on a file as part of your pod deployment requires a write operation. {{site.data.keyword.cos_full_notm}} is not designed for write workloads. Updating permissions during the pod deployment might prevent your pod from getting into a `Running` state.

To resolve this issue, before you mount the PVC to your app pod, create another pod to set the correct permission for the non-root user.

1. Check the permissions of your files in your bucket.
   1. Create a configuration file for your `test-permission` pod and name the file `test-permission.yaml`.
      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Create the `test-permission` pod.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Delete the pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Create a configuration file for the pod that you use to correct the permissions of your files and name it `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Create the `fix-permission` pod.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Wait for the pod to go into a `Completed` state.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Delete the `fix-permission` pod.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Re-create the `test-permission` pod that you used earlier to check the permissions.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verify that the permissions for your files are updated.
   1. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Delete the `test-permission` pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Mount the PVC to the app with the non-root user.

   Define the non-root user as `runAsUser` without setting `fsGroup` in your deployment YAML at the same time. Setting `fsGroup` triggers the {{site.data.keyword.cos_full_notm}} plug-in to update the group permissions for all files in a bucket when the pod is deployed. Updating the permissions is a write operation and might prevent your pod from getting into a `Running` state.
   {: important}

After you set the correct file permissions in your {{site.data.keyword.cos_full_notm}} service instance, do not upload files by using the console or the REST API. Use the {{site.data.keyword.cos_full_notm}} plug-in to add files to your service instance.
{: tip}

<br />

## Object Storage: App pod fails because of an `Operation not permitted` error
{: #cos_operation_not_permitted}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you create a PVC, you see an error message similar to the following:

```
EPERM: operation not permitted
```
{: screen}

{: tsCauses}
IAM has introduced a `refresh_token_expiration` key which causes an issue with the IAM credential response parser, where the parser was not able to differentiate between `expiration` and `refresh_token_expiration`. This issue is resolved in the [community repo](https://github.com/s3fs-fuse/s3fs-fuse/pull/1421) and in the {{site.data.keyword.cos_full_notm}} plug-in.

{: tsResolve}
Follow the steps to [update your {{site.data.keyword.cos_full_notm}} plug-in to the latest version](/docs/containers?topic=containers-object_storage#update_cos_plugin).

## Object Storage: Transport endpoint is not connected
{: cos_transport_ts}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you create a PVC, you see an error message similar to the following:

```sh
Transport endpoint is not connected.
```
{: screen}

{: tsCauses}
To determine the cause of the issue, gather system logs by deploying an `inspectnode` pod.

{: tsResolve}
Follow the steps to gather logging information.

1. Create a `debug-pvc.yaml` file and specify the `ibm.io/debug-level: "info"` and `ibm.io/curl-debug: "true"` annonations.
   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
      annotations:
        ibm.io/auto-create-bucket: "false"
        ibm.io/auto-delete-bucket: "false"
        ibm.io/bucket: <bucket-name> # Enter the name of your bucket.
        ibm.io/secret-name: <cos-secret-name> # Enter the name of your Kubernetes secret that contains your COS credentails
        ibm.io/debug-level: "info"
        ibm.io/curl-debug: "true"
      name: debug-pvc
      namespace: default
   spec:
     storageClassName: ibmc-s3fs-standard-perf-regional
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 10Gi
   ```
   {: codeblock}

2. Create the `debug-pvc` PVC in your cluster.
   ```
   kubectl create -f debug-pvc.yaml>
   ```
   {: pre}

3. Redploy your app and reference the `debug-pvc`. Alternatively, you can use the following sample pod.
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
      name: debug-pod
   spec:
      volumes:
      - name: nginx
         persistentVolumeClaim:
            claimName: cos-debug
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
         - mountPath: /debug
           name: nginx
   ```
   {: codeblock}

4. Save the following daemonset configuration file as `inspectnode-ds.yaml`. After you deploy an app that references the PVC that you created earlier, deploy the daemonset when the `Transport endpoint is not connected.` error occurs to gather the logs.
   ```yaml
   apiVersion: apps/v1
   kind: DaemonSet
   metadata:
      namespace: default
      name: ibm-inspectnode
      labels:
         app: ibm-inspectnode
   spec:
      selector:
         matchLabels:
            app: ibm-inspectnode
      updateStrategy:
         type: RollingUpdate
      template:
         metadata:
            labels:
               app: ibm-inspectnode
         spec:
            tolerations:
            - operator: "Exists"
            hostNetwork: true
            containers:
              - name: inspectnode
               image: alpine:latest
               command: ["/bin/sh"]
               args: ["-c", "while true; do msg=$(date); echo $msg; sleep 30; done"]
               securityContext:
                  runAsUser: 0
               volumeMounts:
                 - mountPath: /host/var/log
                   name: host-log
            volumes:
              - name: host-log
                hostPath:
                  path: /var/log
   ```
   {: codeblock}

5. Create the deamonset in your cluster when the `Transport endpoint is not connected.` error occurs.
   ```sh
   kubectl create -f ./inspectnode-ds.yaml
   ```
   {: pre}

6. Get the name of the `inspectnode` pod that is deployed by the daemonset.
   ```sh
   kubectl get pods -n default -l app=ibm-inspectnode -o wide
   ```
   {: pre}

7. Copy the logs from `inspectnode` pod to your local machine. In the copy command, give your directory a name so that you can identify which node the logs are from. For example, `node-one.log`. You might also use the node IP in the directory name, for example: `10.XXX.XX.XXX.log`.
   ```sh
   kubectl cp <inspectnode_pod_name>:/host/var/log  ./<node_name>.log
   ```
   {: pre}

8. Review the `syslog` and the `s3fslog` for information about the `Transport endpoint` error. [Open a support ticket](/docs/containers?topic=containers-get-help#help-support) and share the log files that you gathered.

9. Delete the `ibm-inspectnode` daemonset that you deployed.
   ```sh
   kubectl delete daemonset ibm-inspectnode
   ```
   {: pre}

## PVC creation fails because of missing permissions
{: #missing_permissions}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you create a PVC, the PVC remains pending. When you run `kubectl describe pvc <pvc_name>`, you see an error message similar to the following:

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster  does not have all the required permissions to provision persistent storage.

{: tsResolve}
1. Retrieve the IAM key or IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster and verify that the correct API key is used.
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}

   Example output:
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}

   The IAM API key is listed in the `Bluemix.iam_api_key` section of your CLI output. If the `Softlayer.softlayer_api_key` is empty at the same time, then the IAM API key is used to determine your infrastructure permissions. The IAM API key is automatically set by the user who runs the first action that requires the IAM **Administrator** platform access role in a resource group and region. If a different API key is set in `Softlayer.softlayer_api_key`, then this key takes precedence over the IAM API key. The `Softlayer.softlayer_api_key` is set when a cluster admin runs the `ibmcloud ks credentials-set` command.

2. If you want to change the credentials, update the API key that is used.
    1.  To update the IAM API key, use the `ibmcloud ks api-key reset` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset). To update the IBM Cloud infrastructure key, use the `ibmcloud ks credential set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set).
    2. Wait about 10 - 15 minutes for the `storage-secret-store` Kubernetes secret to update, then verify that the key is updated.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}

3. If the API key is correct, verify that the key has the correct permission to provision persistent storage.
   1. Contact the account owner to verify the permission of the API key.
   2. As the account owner, select **Manage** > **Access (IAM)** from the navigation in the {{site.data.keyword.cloud_notm}} console.
   3. Select **Users** and find the user whose API key you want to use.
   4. From the actions menu, select **Manage user details**.
   5. Go to the **Classic infrastructure** tab.
   6. Expand the **Account** category and verify that the **Add/ Upgrade Storage (Storage Layer)** permission is assigned.
   7. Expand the **Services** category and verify that the **Storage Manage** permission is assigned.
4. Remove the PVC that failed.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. Re-create the PVC.
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}

## Portworx: Debugging your Portworx installation
{: #debug-portworx}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you create a Portworx service instance from the {{site.data.keyword.cloud_notm}} catalog, the Portworx installation in your cluster fails and the service instance shows a status of **Provision failure**.

{: tsCauses}
Before Portworx is installed in your cluster, a number of checks are performed to verify the information that you provided on the Portworx service page of the {{site.data.keyword.cloud_notm}} catalog. If one of these checks fails, the status of the Portworx service is changed to **Provision failure**. You cannot see the details of what check failed or what information is missing to complete the installation.

{: tsResolve}
Follow this guide to start troubleshooting your Portworx installation and to verify the information that you entered in the {{site.data.keyword.cloud_notm}} catalog. If you find information that you entered incorrectly or you must change the setup of your cluster, correct the information or the cluster setup. Then, create a new Portworx service instance to restart the installation.

### Step 1: Verifying the {{site.data.keyword.cloud_notm}} catalog information
{: #px-verify-catalog}

Start by verifying that the information that you entered in the {{site.data.keyword.cloud_notm}} catalog is correct. If information was entered incorrectly, the installation does not pass the pre-installation checks and fails without starting the installation.
{: shortdesc}

1. Verify that the cluster where you want to install Portworx is located in the {{site.data.keyword.cloud_notm}} region and resource group that you selected.
   ```
   ibmcloud ks cluster get --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Verify that the {{site.data.keyword.cloud_notm}} API key that you entered has sufficient permissions to work with your cluster. You must be assigned the **Editor** platform access role and the **Manager** service access role for {{site.data.keyword.containerlong_notm}}. For more information, see [User access permissions](/docs/containers?topic=containers-access_reference).
3. Verify that you entered the `etcd` API endpoint for your Databases for etcd service instance in the correct format.  
   1. [Retrieve the Databases for etcd endpoint](/docs/containers?topic=containers-portworx#databases_credentials).
   2. Add the etcd endpoint in the format `etcd:<etcd_endpoint1>;etcd:<etcd_endpoint2>`. If you have more than one endpoint, include all endpoints and separate them with a semicolon (;).

      Example endpoint:
      ```
      etcd:https://1ab234c5-12a1-1234-a123.databases.appdomain.cloud:32059
      ```
      {: screen}
4. Verify that you stored the credentials to access your Databases for etcd service instance in a Kubernetes secret in your cluster. For more information, see [Setting up a Databases for etcd service instance for Portworx metadata](/docs/containers?topic=containers-portworx#portworx_database).
   1. Review steps 4-6 and verify that you retrieved the correct username, password, and certificate.
   2. List the secrets in your cluster and look for the secret that holds the credentials of your Databases for etcd service instance.
      ```
      kubectl get secrets
      ```
      {: pre}
   3. Make sure that the username, password, and certificate are stored as a base64 encoded value in your Kubernetes secret.
   4. Verify that you entered the correct name of the secret in the {{site.data.keyword.cloud_notm}} catalog.
5. If you chose to set up volume encryption with {{site.data.keyword.keymanagementservicelong_notm}}, make sure that you created an instance of {{site.data.keyword.keymanagementservicelong_notm}} in your {{site.data.keyword.cloud_notm}} account and that you stored the credentials to access your instance in a Kubernetes secret in the `portworx` namespace of your cluster. For more information, see [Enabling per-volume encryption for your Portworx volumes](/docs/containers?topic=containers-portworx#setup_encryption).
   1. Make sure that the API key of your service ID, and the {{site.data.keyword.keymanagementservicelong_notm}} instance ID, root key, and API endpoint are stored as base64 values in the Kubernetes secret of your cluster.
   2. Make sure that you named your secret `px-ibm`.
   3. Make sure that you created the secret in the `portworx` namespace of your cluster.

### Step 2: Verifying the cluster setup
{: #px-verify-cluster}

If you entered the correct information on the {{site.data.keyword.cloud_notm}} catalog page, verify that your cluster is correctly set up for Portworx.
{: shortdesc}

1. Verify that the cluster that you want to use meets the [minimum hardware requirements for Portworx](https://docs.portworx.com/start-here-installation/){: external}.
2. If you want to use a virtual machine cluster, make sure that you [added raw, unformatted, and unmounted block storage](/docs/containers?topic=containers-portworx#create_block_storage) to your cluster so that Portworx can include the disks into the Portworx storage layer.
3. Verify that your cluster is set up with public network connectivity. For more information, see [Understanding network basics of classic clusters](/docs/containers?topic=containers-plan_clusters#plan_basics).



### Step 3: Reach out to Portworx and IBM
{: #px-support}

If you run into an issue with using Portworx, you can open an issue in the [Portworx Service Portal](https://portworx.atlassian.net/servicedesk/customer/portal/2){: external}. You can also submit a request by sending an e-mail to `support@portworx.com`. If you do not have an account on the Portworx Service Portal, send an e-mail to `support@portworx.com`.

## Portworx: Encryption set up fails due to invalid KMS endpoint
{: #px-kms-endpoint}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
* <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you provision Portworx and set up encryption, you receive an error similar to the following:
```
`kp.Error: correlation_id='673bb68a-be17-4720-9ae1-85baf109924e', msg='Unauthorized: The user does not have access to the specified resource'"`
```
{: screen}

{: tsCauses}
The endpoint that you entered in your Kubernetes secret is incorrect. If the KMS endpoint is entered incorrectly, Portworx cannot access the KMS provider that you configured. For more information about enabling encryption on your Portworx volumes, see [Setting up encryption](/docs/openshift?topic=openshift-portworx#encrypt_volumes).

{: tsResolve}
1. Retrieve the correct endpoint for your KMS provider.

   * **{{site.data.keyword.keymanagementservicelong_notm}}**: [Retrieve the region](/docs/key-protect?topic=key-protect-regions#regions) where you created your service instance. Make sure that you note your API endpoint in the correct format. Example: `https://us-south.kms.cloud.ibm.com`.
   * **{{site.data.keyword.hscrypto}}**: [Retrieve the Key Management public endpoint URL](/docs/hs-crypto?topic=hs-crypto-regions#service-endpoints). Make sure that you note your endpoint in the correct format. Example: `https://api.us-south.hs-crypto.cloud.ibm.com:<port>`. For more information, see the [{{site.data.keyword.hscrypto}} API documentation](https://cloud.ibm.com/apidocs/hs-crypto#getinstance).

2. Encode the endpoint to base64.
   ```sh
   echo -n "<endpoint>" | base64
   ```
   {: pre}

3. [Edit the Kubernetes secret that you created to include the correct endpoint for your KMS provider](/docs/openshift?topic=openshift-portworx#creating-a-secret-to-store-the-kms-credentials).
   ```sh
   kubectl edit <secret-name> -n portworx
   ```
   {: pre}

4. Save and close your Kubernetes secret to reapply it to your cluster.


If you find information that you entered incorrectly or you must change the setup of your cluster, correct the information or the cluster setup.

## Feedback, questions, and support
{: #getting_help_storage}

If you still experience issues with persistent storage in your cluster, review the following options to receive further support or ask questions.

- For issues with {{site.data.keyword.cloud_notm}} File, Block, or Object Storage, see [Getting help](/docs/containers?topic=containers-get-help) to find information about how to contact the IBM team on Slack or open an {{site.data.keyword.cloud_notm}} support case.
- For issues with Portworx, open an issue in the [Portworx Service Portal](https://portworx.atlassian.net/servicedesk/customer/portal/2){: external}. You can also submit a request by sending an e-mail to `support@portworx.com`. If you do not have an account on the Portworx Service Portal, send an e-mail to `support@portworx.com`.


