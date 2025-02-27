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
 


# Managing the app lifecycle
{: #update_app}

Use Kubernetes-native and third-party tools to perform rolling updates and rollbacks of apps without downtime for your users.
{: shortdesc}

## Update strategies
{: #updating_apps}

To update your app, you can choose from various strategies such as the following. You might start with a rolling deployment or instantaneous switch before you progress to a more complicated canary deployment.

<dl>
<dt>Rolling deployment</dt>
  <dd>You can use Kubernetes-native functionality to create a `v2` deployment and to gradually replace your previous `v1` deployment. This approach requires that apps are backwards-compatible so that users who are served the `v2` app version do not experience any breaking changes. For more information, see [Managing rolling deployments to update your apps](/docs/containers?topic=containers-update_app#app_rolling).</dd>
<dt>Instantaneous switch</dt>
  <dd>Also referred to as a blue-green deployment, an instantaneous switch requires double the compute resources to have two versions of an app running at once. With this approach, you can switch your users to the newer version in near real time. Make sure that you use service label selectors (such as `version: green` and `version: blue`) to make sure that requests are sent to the right app version. You can create the new `version: green` deployment, wait until it is ready, and then delete the `version: blue` deployment. Or you can perform a [rolling update](/docs/containers?topic=containers-update_app#app_rolling), but set the `maxUnavailable` parameter to `0%` and the `maxSurge` parameter to `100%`.</dd>
<dt>Canary or A/B deployment</dt>
  <dd>A more complex update strategy, a canary deployment is when you pick a percentage of users such as 5% and send them to the new app version. You collect metrics in your logging and monitoring tools on how the new app version performs, do A/B testing, and then roll out the update to more users. As with all deployments, labeling the app (such as `version: stable` and `version: canary`) is critical. To manage canary deployments, you might [install the managed Istio add-on service mesh](/docs/containers?topic=containers-istio), [set up Sysdig monitoring for your cluster](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-kubernetes_cluster#kubernetes_cluster), and then use the Istio service mesh for A/B testing as described [in this blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://sysdig.com/blog/monitor-istio/).</dd>
</dl>


## Scaling apps
{: #app_scaling}

With Kubernetes, you can enable [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/){: external} to automatically increase or decrease the number of instances of your apps based on CPU.
{: shortdesc}

Want to scale your worker nodes instead of your pods? Check out the [cluster autoscaler](/docs/containers?topic=containers-ca#ca).
{: tip}

Before you begin:
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Make sure that you are assigned a [service access role](/docs/containers?topic=containers-users#platform) that grants the appropriate Kubernetes RBAC role so that you can work with Kubernetes resources in the namespace.

Steps:

1.  Deploy your app to a cluster from the CLI. For more complex deployments, you might need to create a [configuration file](/docs/containers?topic=containers-deploy_app#app_cli).
    ```
    kubectl create deployment <app_name> --image=<image>
    ```
    {: pre}
2.  Set CPU resource limits in millicores for the containers that run in your deployment, such as `100m`. You can also set memory limits, but the horizontal pod autoscaler only considers CPU resource limits for scaling purposes. For more information, see the [`kubectl set resources` documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-resources-em-){: external}.
    ```
    kubectl set resources deployment <app_name> --limits=cpu=100m
    ```
    {: pre}
3.  Create a horizontal pod autoscaler and define your policy. For more information, see the [`kubectl autoscale` documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale){: external}.

    ```
    kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table summary="A table that describes in Column 1 the kubectl command options and in Column 2 how to fill out those options.">
    <caption>Command components for `kubectl autoscale`</caption>
    <thead>
    <col width="25%">
    <th>Component</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>The average CPU utilization that is maintained by the Horizontal Pod Autoscaler, which is specified as a percentage.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>The minimum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>The maximum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    </tbody></table>


<br />

## Managing rolling deployments to update your apps
{: #app_rolling}

You can manage the rollout of your app changes in an automated and controlled fashion for workloads with a pod template such as deployments. If your rollout isn't going according to plan, you can roll back your deployment to the previous revision.
{: shortdesc}

Want to prevent downtime during your rolling update? Be sure to specify a [readiness probe in your deployment](/docs/containers?topic=containers-app#probe) so that the rollout proceeds to the next app pod after the most recently updated pod is ready.
{: tip}

Before you begin:
*   [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Create a [deployment](/docs/containers?topic=containers-deploy_app#app_cli).
*   Make sure that you have a [service access role](/docs/containers?topic=containers-users#platform) that grants the appropriate Kubernetes RBAC role so that you can work with Kubernetes resources in the namespace.

To manage rolling updates to your apps:
1.  To make sure that your deployments are marked as ready only when the container is running and ready to service requests, add [liveness and readiness probes to your deployment](/docs/containers?topic=containers-app#probe).

2.  Update your deployment to include a rolling update strategy that specifies the maximum surge and unavailable pods or percentage of pods during the update.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:  
      name: nginx-test
    spec:
      replicas: 10
      selector:
        matchLabels:
          service: http-server
      minReadySeconds: 5
      progressDeadlineSeconds: 600
      strategy: 
        type: RollingUpdate  
        rollingUpdate:    
          maxUnavailable: 50%
          maxSurge: 2
    ...
    ```
    {: codeblock}

    <table summary="A table that describes in Column 1 the YAML file fields and in Column 2 how to fill out those fields.">
    <caption>YAML components</caption>
    <thead>
    <col width="25%">
    <th>Component</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code>spec.minReadySeconds</code></td>
    <td>By default, deployments wait until the pod is marked as `ready` to continue with the rollout. If you notice that the deployment continues to create pods even though your app in the most recent pod is not yet ready, use this field to slow down the deployment rollout. For example, if you specify `5`, the deployment waits for 5 seconds after the pod is `ready` before it creates the next pod.</td>
    </tr>
    <tr>
    <td><code>spec.progressDeadlineSeconds</code></td>
    <td>Set a timeout in seconds before a deployment is considered failed. For example, without a timeout, if your new app version has a bug and hangs immediately, the rollout cannot continue because the pod never reaches a `ready` state. If you set this timeout to `600` seconds, then if any phase of the rollout fails to progress for 10 minutes, the deployment is marked as failed and the rollout stops.</td>
    </tr>
    <tr>
    <td><code>spec.strategy.type</code></td>
    <td>Specify the `RollingUpdate` strategy type.</td>
    </tr>
    <tr>
    <td><code>spec.strategy.rollingUpdate.maxUnavailable</code></td>
    <td>Set the maximum number of pods that can be unavailable during an update, as a number (`2`) or percentage (`50%`). Generally, use a percentage so that if you change the number of replicas later you don't have to remember to update the number here, unless you want to limit the rollout to allow only one pod to be down at a time. If you never want to fall below 100% capacity, set this value to `0%` and specify the `spec.strategy.type.rollingUpdate.maxSurge` parameter.</td>
    </tr>
    <tr>
    <td><code>spec.strategy.rollingUpdate.maxSurge</code></td>
    <td>Set how many extra resources the deployment can use during the rollout, as a number (`2`) or percentage (`50%`). For example, if your deployment specifies `10` replicas and you set the `maxSurge` to `2`, then during the rollout, two new replicas are created. You now have 12 replicas (10 existing, 2 new). After the two new replicas are ready, the deployment scales down the old replicas to 8 to meet the specified 10 replicas. This process continues until the rollout is complete and all 10 replicas run the new version.<p class="tip">If you want to perform a blue-green instantaneous switch style update, set the `maxSurge` to `100%`. The deployment creates all the new required replicas, then scales down the old version replicas to 0.</p></td>
    </tr>
    </tbody></table>

3.  [Roll out](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment){: external} a change. For example, you might want to change the image that you used in your initial deployment.

    1.  Get the deployment name.

        ```
        kubectl get deployments
        ```
        {: pre}

    2.  Get the pod name.

        ```
        kubectl get pods
        ```
        {: pre}

    3.  Get the name of the container that runs in the pod.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  Set the new image for the deployment to use.

        ```
        kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    When you run the commands, the change is immediately applied and logged in the roll-out history.

4.  Check the status of your deployment.

    ```
    kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

    If you notice something in the status that you want time to follow up on, you can pause and resume your rollout with the following commands.

    ```
    kubectl rollout pause deployment <deployment_name>
    ```
    {: pre}

    ```
    kubectl rollout resume deployment <deployment_name>
    ```
    {: pre}

5.  Roll back a change.
    1.  View the roll-out history for the deployment and identify the revision number of your last deployment.

        ```
        kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **Tip:** To see the details for a specific revision, include the revision number.

        ```
        kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  Roll back to the previous version, or specify a revision. To roll back to the previous version, use the following command.

        ```
        kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />

## Setting up continuous integration and delivery
{: #app_cicd}

With {{site.data.keyword.cloud_notm}} and other open source tools, you can set up continuous integration and delivery (CI/CD), version control, tool chains, and more to help automate app development and deployment.
{: shortdesc}

To see a list of supported integrations and steps for setting up a continuous delivery pipeline, see [Setting up continuous integration and delivery](/docs/containers?topic=containers-cicd).

<br />

## Copying deployments to another cluster
{: #copy_apps_cluster}

When you use a [version control system such as Git](/docs/containers?topic=containers-plan_deploy#deploy_organize), configuration management projects such as [`kustomize`](/docs/containers?topic=containers-app#kustomize), or continuous delivery tools such as [Razee](https://razee.io/){: external} in your cluster, you can deploy your app configuration files quickly from cluster to cluster. Sometimes you have only a few deployments that you tested in a cluster and prefer to copy these deployments and redeploy in another cluster. For example, you might use a free, classic infrastructure Kubernetes cluster for a proof of concept that you did not manage in Git. Now, you are ready to take this proof of concept and deploy it to a standard cluster that runs {{site.data.keyword.openshiftshort}} or on VPC infrastructure.
{: shortdesc}

Before you begin, you need two clusters and the **Manager** [service access role](/docs/containers?topic=containers-users#platform) for all namespaces in both clusters so that you can copy all the resources from one cluster and deploy them to another.

1.  [Target](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) the cluster that you want to copy resources from, such as a free cluster.
2.  List all the configuration files in your cluster and verify that you want to copy these configurations.
    ```
    kubectl get all
    ```
    {: pre}

    Example output:
    ```
    NAME                                   READY   STATUS             RESTARTS   AGE
    pod/java-web-6955bdbcdf-l756b          1/1     Running            0          59d

    NAME                                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    service/java-web                       NodePort    172.21.xxx.xxx   <none>        8080:30889/TCP   59d

    NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/java-web               1/1     1            1           59d

    NAME                                   DESIRED   CURRENT   READY   AGE
    replicaset.apps/java-web-6955bdbcdf    1         1         1       59d
    ```
    {: screen}
3.  Copy the configuration files in your cluster to a local directory. The `--export` flag removes cluster-specific information from the configuration files.
    ```
    kubectl get all -o yaml --export > myconfigs.yaml
    ```
    {: pre}
4.  [Target](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) the cluster that you want to copy the resources to, such as a production-ready standard cluster.
5.  Optional: If your free cluster used multiple namespaces, create the same namespaces in the standard cluster and [copy the image pull secret to each namespace](/docs/containers?topic=containers-registry#copy_imagePullSecret).
6.  Deploy the copied configuration files to your cluster. If a configuration file has specific information that cannot be applied, you might need to update the configuration file and reapply.
    ```
    kubectl apply -f myconfigs.yaml
    ```
    {: pre}
    Example output:
    ```
    pod/java-web-6955bdbcdf-l756b created
    service/java-web created
    deployment.apps/java-web created
    replicaset.apps/java-web-6955bdbcdf created
    ```
    {: screen}
7.  Verify that your configuration files are applied.
    ```
    kubectl get all
    ```
    {: pre}


