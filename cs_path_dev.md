---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: kubernetes, iks, containers

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
 


<style>
    <!--
        #tutorials { /* hide the page header */
            display: none !important;
        }
        .allCategories {
            display: flex !important;
            flex-direction: row !important;
            flex-wrap: wrap !important;
        }
        .categoryBox {
            flex-grow: 1 !important;
            width: calc(33% - 20px) !important;
            text-decoration: none !important;
            margin: 0 10px 20px 0 !important;
            padding: 16px !important;
            border: 1px #dfe6eb solid !important;
            box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2) !important;
            text-align: center !important;
            text-overflow: ellipsis !important;
            overflow: hidden !important;
        }
        .solutionBoxContainer {}
        .solutionBoxContainer a {
            text-decoration: none !important;
            border: none !important;
        }
        .solutionBox {
            display: inline-block !important;
            width: 100% !important;
            margin: 0 10px 20px 0 !important;
            padding: 16px !important;
            background-color: #f4f4f4 !important;
        }
        @media screen and (min-width: 960px) {
            .solutionBox {
            width: calc(50% - 3%) !important;
            }
            .solutionBox.solutionBoxFeatured {
            width: calc(50% - 3%) !important;
            }
            .solutionBoxContent {
            height: 350px !important;
            }
        }
        @media screen and (min-width: 1298px) {
            .solutionBox {
            width: calc(33% - 2%) !important;
            }
            .solutionBoxContent {
            min-height: 350px !important;
            }
        }
        .solutionBox:hover {
            border: 1px rgb(136, 151, 162)solid !important;
            box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2) !important;
        }
        .solutionBoxContent {
            display: flex !important;
            flex-direction: column !important;
        }
        .solutionBoxTitle {
            margin: 0rem !important;
            margin-bottom: 5px !important;
            font-size: 14px !important;
            font-weight: 900 !important;
            line-height: 16px !important;
            height: 37px !important;
            text-overflow: ellipsis !important;
            overflow: hidden !important;
            display: -webkit-box !important;
            -webkit-line-clamp: 2 !important;
            -webkit-box-orient: vertical !important;
            -webkit-box-align: inherit !important;
        }
        .solutionBoxDescription {
            flex-grow: 1 !important;
            display: flex !important;
            flex-direction: column !important;
        }
        .descriptionContainer {
        }
        .descriptionContainer p {
            margin: 0 !important;
            overflow: hidden !important;
            display: -webkit-box !important;
            -webkit-line-clamp: 4 !important;
            -webkit-box-orient: vertical !important;
            font-size: 14px !important;
            font-weight: 400 !important;
            line-height: 1.5 !important;
            letter-spacing: 0 !important;
            max-height: 70px !important;
        }
        .architectureDiagramContainer {
            flex-grow: 1 !important;
            min-width: calc(33% - 2%) !important;
            padding: 0 16px !important;
            text-align: center !important;
            display: flex !important;
            flex-direction: column !important;
            justify-content: center !important;
            background-color: #f4f4f4;
        }
        .architectureDiagram {
            max-height: 175px !important;
            padding: 5px !important;
            margin: 0 auto !important;
        }
    -->
    </style>

# Learning path for developers
{: #learning-path-dev}

Following a curated learning path to deploy highly available containerized apps in Kubernetes clusters and use the powerful tools of {{site.data.keyword.containerlong_notm}} to automate, isolate, secure, manage, and monitor your app workloads across zones or regions.
{: shortdesc}

<div class=solutionBoxContainer>
  <div class="solutionBox">
    <a href = "#dev_cluster">
      <div>
        <img src="images/desktop.svg" alt="Access icon" style="height:50px; border-style: none"/>
        <h2>Access the cluster</h2>
        <p class="bx--type-caption">Begin working with your cluster by setting up the CLI and accessing the cluster.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_plan">
      <div>
        <img src="images/progress.svg" alt="Plan icon" style="height:50px; border-style: none"/>
        <h2>Plan your deployment</h2>
        <p class="bx--type-caption">Plan your app setup for optimal service integration and high availability.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_develop">
      <div>
        <img src="images/design--and--development--02.svg" alt="Develop icon" style="height:50px; border-style: none"/>
        <h2>Develop your app</h2>
        <p class="bx--type-caption">Configure your app and set up your app versioning and delivery pipeline.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_deploy">
      <div>
        <img src="images/app--developer.svg" alt="Deploy icon" style="height:50px; border-style: none"/>
        <h2>Deploy your app</h2>
        <p class="bx--type-caption">Deploy your app to the cluster by running your app configuration file.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_test">
      <div>
        <img src="images/chart--line.svg" alt="Health icon" style="height:50px; border-style: none"/>
        <h2>Test, log, and monitor</h2>
        <p class="bx--type-caption">Conduct app performance testing and gain visibility into your workload health.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_update">
      <div>
        <img src="images/networking--04.svg" alt="Update icon" style="height:50px; border-style: none"/>
        <h2>Update your app</h2>
        <p class="bx--type-caption">Perform rolling updates and rollbacks of apps without downtime for your users.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_secure">
      <div>
        <img src="images/lock--alt.svg" alt="Security icon" style="height:50px; border-style: none"/>
        <h2>Secure your app</h2>
        <p class="bx--type-caption">Encrypt data and store confidential information in Kubernetes secrets.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_expose">
      <div>
        <img src="images/global--network.svg" alt="Network icon" style="height:50px; border-style: none"/>
        <h2>Expose your app</h2>
        <p class="bx--type-caption">Expose an app to users on the internet or on a private network only.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_storage">
      <div>
        <img src="images/data--storage.svg" alt="Storage icon" style="height:50px; border-style: none"/>
        <h2>Add app storage</h2>
        <p class="bx--type-caption">Plan and add highly available persistent storage for your app data.</p>
      </div>
    </a>
  </div>
  <div class="solutionBox">
    <a href = "#dev_integrate">
      <div>
        <img src="images/connect.svg" alt="Integrations icon" style="height:50px; border-style: none"/>
        <h2>Add integrations</h2>
        <p class="bx--type-caption">Enhance app capabilities by integrating external and catalog services.</p>
      </div>
    </a>
  </div>
</div>

## Access the cluster
{: #dev_cluster}

Begin working with your cluster by setting up the CLI and accessing the cluster.
{: shortdesc}

1. **CLI setup**: [Set up the CLIs](/docs/containers?topic=containers-cs_cli_install) that are necessary to create and work with clusters. As you work with your cluster, refer to the [command reference](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) and keep track of CLI version updates with the [CLI changelog](/docs/containers?topic=containers-cs_cli_changelog).
2. **User permissions**: Ensure that your cluster administrator gives you the proper [{{site.data.keyword.cloud_notm}} IAM role](/docs/containers?topic=containers-learning-path-admin#admin_secure) to access the cluster.
3. **Cluster access**: [Access your cluster through the public or private cloud service endpoint](/docs/containers?topic=containers-access_cluster).

</br>Need help? Check out [Troubleshooting clusters and masters](/docs/containers?topic=containers-cs_troubleshoot) and [Troubleshooting worker nodes](/docs/containers?topic=containers-cs_troubleshoot_clusters).

<br />

## Plan your deployment
{: #dev_plan}

Before you deploy an app, decide how you want to set up your app so that your app can be accessed properly and be integrated with other services.
{: shortdesc}

1. **Kubernetes-native**: [Plan your strategy for developing a Kubernetes-native app](/docs/containers?topic=containers-plan_deploy).
2. **Highly available**: [Plan your strategy for a highly available deployment](/docs/containers?topic=containers-plan_deploy#highly_available_apps).

<br />

## Develop your app
{: #dev_develop}

Configure your app in a YAML file that declares the configuration of the Kubernetes object, and plan your app versioning strategy.
{: shortdesc}

1. **Develop your app**:
  1. Review the [basics of Kubernetes-native app deployments](/docs/containers?topic=containers-plan_deploy#kube-objects).
  2. Build app containers from [images in public or private image registries](/docs/containers?topic=containers-images).
  3. Specify your [app requirements in a YAML file](/docs/containers?topic=containers-app#app_yaml), which declares the configuration of the Kubernetes object.
2. **Version your app**:
  1. To plan customized configurations for more than one environment, such as development, testing, and production environments, [use the Kustomize tool](/docs/containers?topic=containers-app#kustomize) to manage your configuration YAML file.
  2. If you want to run your app in multiple clusters, public and private environments, or even multiple cloud providers, [package your application to help automate deployments](/docs/containers?topic=containers-plan_deploy#packaging).

</br>Need help? Check out [Troubleshooting apps and integrations](/docs/containers?topic=containers-cs_troubleshoot_app).

<br />

## Deploy your app
{: #dev_deploy}

Deploy your app to the cluster by running your app configuration file.
{: shortdesc}


* [Deploying apps with the Kubernetes dashboard](/docs/containers?topic=containers-deploy_app#app_ui)
* [Deploying apps with the CLI](/docs/containers?topic=containers-deploy_app#app_cli)
* [Deploying apps to specific worker nodes by using labels](/docs/containers?topic=containers-deploy_app#node_affinity)
* [Deploying an app on a GPU machine](/docs/containers?topic=containers-deploy_app#gpu_app)


</br>Need help? Check out [Troubleshooting apps and integrations](/docs/containers?topic=containers-cs_troubleshoot_app).

<br />

## Test, log, and monitor
{: #dev_test}

While you conduct performance testing on your app, set up logging and monitoring to help you troubleshoot issues, gain visibility into your workloads, and improve the health and performance of your apps.
{: shortdesc}

In a test environment, deliberately create various non-ideal scenarios, such as deleting all worker nodes in a zone to replicate a zonal failure. Review the logs and metrics to check how your app recovers.

1. **Test access**: Test access to your app by creating a public or private [NodePort](/docs/containers?topic=containers-nodeport) on your worker nodes.
2. **Monitoring**:
  1. Open a [Kubernetes dashboard](/docs/containers?topic=containers-deploy_app#cli_dashboard) on your local system to view information about your app resources.
  2. [Choose a monitoring solution](/docs/containers?topic=containers-health-monitor#view_metrics), such as {{site.data.keyword.mon_full}}, to gain operational visibility into the performance and health of your apps.
3. **Logging**:
  1. [Choose a logging solution](/docs/containers?topic=containers-health#logging_overview), such as {{site.data.keyword.la_full}}, to monitor container logs.
  2. If you expose your app by using [Ingress](#dev_expose), you can set up [logging for requests that are proxied by your ALBs](/docs/containers?topic=containers-ingress_health).

</br>Need help? Check out [Troubleshooting logging and monitoring](/docs/containers?topic=containers-cs_troubleshoot_health#cs_troubleshoot_health).

<br />

## Update your app
{: #dev_update}

Perform rolling updates and rollbacks of apps without downtime for your users.
{: shortdesc}

1. **Update strategy**: [Plan your strategy for keeping your app up-to-date](/docs/containers?topic=containers-update_app#updating_apps).
2. **Set up updates**:
  * Add a [rolling update to your deployment file](/docs/containers?topic=containers-update_app#app_rolling)
  * Perform A/B, canary, and phased rollouts with the [Istio managed add-on](/docs/containers?topic=containers-istio-qs).
  * Set up [a continuous delivery pipeline for a cluster](/docs/containers?topic=containers-cicd).
3. **Scaling**: Enable [horizontal pod autoscaling](/docs/containers?topic=containers-update_app#app_scaling) to automatically increase or decrease the number of instances of your apps based on CPU.

<br />

## Secure your app
{: #dev_secure}

Use Kubernetes secrets to store confidential information, such as credentials or keys, and encrypt data in Kubernetes secrets to prevent unauthorized users from accessing sensitive app information.
{: shortdesc}


1. **Secrets**:
  1. Store personal or sensitive information in [Kubernetes secrets](/docs/containers?topic=containers-security#pi) that your app can access.
  2. [Encrypt secrets by using a KMS provider](/docs/containers?topic=containers-encryption#keyprotect).
  3. [Verify that secrets are encrypted](/docs/containers?topic=containers-encryption#verify_kms).
2. **Pod-to-pod traffic**: [Enable mTLS encryption for traffic between microservices within an Istio service mesh](/docs/containers?topic=containers-istio-mesh#mtls).

<br />

## Expose your app
{: #dev_expose}

Publicly expose an app in your cluster to the internet or privately expose an app in your cluster to the private network only.
{: shortdesc}

1. **Plan service discovery**:
  1. Understand the [basics of Kubernetes service discovery](/docs/containers?topic=containers-plan_deploy#service_discovery).
  2. [Choose an app exposure service](/docs/containers?topic=containers-cs_network_planning) that fits your requirements for incoming traffic to the app.
2. **Expose your app**:
  * Load balancers:
    * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic clusters:
        1. Create an [NLB 1.0](/docs/containers?topic=containers-loadbalancer) or [NLB 2.0](/docs/containers?topic=containers-loadbalancer-v2).
        2. [Register a DNS subdomain](/docs/containers?topic=containers-loadbalancer_hostname#loadbalancer_hostname) for the NLB.
    * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC clusters: Set up a [VPC load balancer](/docs/containers?topic=containers-vpc-lbaas).

  * Ingress:
      * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic clusters: Configure Ingress for the [public network](/docs/containers?topic=containers-ingress#ingress_expose_public) or the [private network](/docs/containers?topic=containers-ingress#ingress_expose_private).
      * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC clusters: Configure Ingress for the [public network](/docs/containers?topic=containers-ingress#ingress_expose_public) or the [private network](/docs/containers?topic=containers-ingress#ingress_expose_vpc_private).

</br>Need help? Check out [Troubleshooting Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress) and [Troubleshooting load balancers](/docs/containers?topic=containers-cs_troubleshoot_lb).

<br />

## Add app storage
{: #dev_storage}

1. **Storage basics**: Start by understanding the [basics of Kubernetes storage](/docs/containers?topic=containers-kube_concepts).
2. **Requirements**: Determine your [requirements for a storage solution](/docs/containers?topic=containers-storage_planning).
3. **Choose a solution**: Using your storage requirements, choose a storage solution by comparing [non-persistent](/docs/containers?topic=containers-storage_planning#non_persistent_overview), [single-zone persistent](/docs/containers?topic=containers-storage_planning#single_zone_persistent_storage), or [multizone persistent](/docs/containers?topic=containers-storage_planning#persistent_storage_overview) storage.

</br>Need help? Check out [Troubleshooting persistent storage](/docs/containers?topic=containers-cs_troubleshoot_storage#cs_troubleshoot_storage).

<br />

## Add integrations
{: #dev_integrate}

Enhance app capabilities by integrating various external services and catalog services in your cluster with your app.
{: shortdesc}

1. **Review supported integrations**:
  * [All supported integrations](/docs/containers?topic=containers-supported_integrations#supported_integrations)
  * [{{site.data.keyword.containerlong_notm}} partners](/docs/containers?topic=containers-service-partners)
  * [{{site.data.keyword.cloud_notm}} services and third-party integrations](/docs/containers?topic=containers-ibm-3rd-party-integrations)
2. **Add services to your cluster**: Ask your cluster administrator to [add the integration to your cluster](/docs/containers?topic=containers-learning-path-admin#admin_integrate).
3. **Access services from your app**: Ensure that your app can access the service. For example, to access an IBM Cloud service instance from your app, you must [make the service credentials that are stored in the Kubernetes secret available to your app](/docs/containers?topic=containers-service-binding#adding_app).

</br>Need help? Check out [Troubleshooting apps and integrations](/docs/containers?topic=containers-cs_troubleshoot_clusters).


