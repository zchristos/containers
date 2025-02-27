---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-22"

keywords: vpc block, add-on, vpc block changelog

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
 

# {{site.data.keyword.block_storage_is_short}} add-on changelog
{: #vpc_bs_changelog}

View information for patch updates to the {{site.data.keyword.block_storage_is_full}} add-on in your {{site.data.keyword.containerlong_notm}} clusters.
{: shortdesc}

* **Patch updates**: Patch updates are delivered automatically by IBM and do not contain any feature updates or changes in the supported add-on and cluster versions.
* **Release updates**: Release updates contain new features for the {{site.data.keyword.block_storage_is_full}} or changes in the supported add-on or cluster versions. You must manually apply release updates to your {{site.data.keyword.block_storage_is_full}} add-on. To update your {{site.data.keyword.block_storage_is_full}} add-on, see [Updating the {{site.data.keyword.block_storage_is_full}} add-on](/docs/containers?topic=containers-vpc-block#vpc-addon-update).

Version `2.0.2` of {{site.data.keyword.block_storage_is_short}} add-on is unsupported. Version `2.0.3` is deprecated and no longer receives fixes or updates. If your cluster runs a deprecated or unsupported add-on version, update your cluster to the latest version. You can update the add-on by disabling and re-enabling it. Disable the add-on by running the `ibmcloud ks cluster addon disable vpc-block-csi-driver --cluster <cluster-name>` command. Then, re-enable by running the `ibmcloud ks cluster addon enable vpc-block-csi-driver --cluster <cluster-name>` command. Existing PVs and PVCs are not impacted. For more information, see [updating the {{site.data.keyword.block_storage_is_short}} add-on](/docs/containers?topic=containers-vpc-block#vpc-addon-update)
{: important}

Refer to the following tables for a summary of changes for each version of the {{site.data.keyword.block_storage_is_full}} add-on.

| {{site.data.keyword.block_storage_is_full}} add-on version | Supported? | {{site.data.keyword.containerlong_notm}} version support |
| -------------------- | -----------|--------------------------- |
| 3.0.0 | <img src="images/icon-checkmark-confirm.svg" width="32" alt="Supported" style="width:32px;" /> | 1.15 - 1.20 |
| 2.0.3 | | 1.15 - 1.20 |
{: summary="The rows are read from left to right. The first column is the {{site.data.keyword.block_storage_is_full}} add-on version. The second column is the version's supported state. The third column is the Kubernetes version of your cluster that the {{site.data.keyword.block_storage_is_full}} version is supported for."}

## Changelog for version 3.0.0
{: #0300_is_block}

The following table shows the changes that are included in version 3.0.0 {{site.data.keyword.block_storage_is_full}} add-on.
{: shortdesc}

To view a list of add-ons and the supported Kubernetes versions, run the following command.
```
ibmcloud ks addon-versions
```
{: pre}

| Patch version | `vpc-block-csi-driver` image tag | Release date | Supported Kubernetes versions | Description |
| --- | --- | --- | --- | --- |
| N/A | `v.3.0.0` | 26 February 2021 | 1.15 - 1.20 | The `vpc-block-csi` driver is now available for both managed clusters and unmanaged clusters. This release contains no functional changes. |
{: row-headers}
{: class="comparison-table"}
{: caption="Patch updates for version 3.0.0" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the patch version number of the component. The second column contains the image tag the component. The third column contains the release date of the patch. The fourth column contains the supported cluster versions. The fifth column contains a brief description of the change made to the component."}

## Changelog for version 2.0.3
{: #0203_is_block}

The following table shows the changes that are included in version 2.0.3 {{site.data.keyword.block_storage_is_full}} add-on.
{: shortdesc}

To view a list of add-ons and the supported Kubernetes versions, run the following command.
```
ibmcloud ks addon-versions
```
{: pre}

| Patch version | `vpc-block-csi-driver` image tag | Release date | Supported Kubernetes versions | Description |
| --- | --- | --- | --- | --- |
| `2.0.3_471` | `v.2.0.9` | 26 January 2021 | 1.15 - 1.20 | Includes fixes for vulnerability scan issues. The `openssl`, `openssl-libs`, `gnutls` packages are updated to fix [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external} and [CVE-2020-24659](https://nvd.nist.gov/vuln/detail/CVE-2020-24659){: external}. |
| `2.0.3_464` | `v2.0.8` | 10 December 2020 | 1.15 - 1.20 | Updates in this patch:<ul><li>New metro storage classes with the `volumeBindingMode:WaitForFirstConsumer` specification.</li><li>Resources that are deployed by the add-on now contain a label which links the source code URL and the build URL.</li><li>The `v2.0.8` image is signed.</li><li>Updates the Go version from `1.15.2` to `1.15.5`.</li></ul> |
| `2.0.3_404` | `v2.0.7` | 25 November 2020 | 1.15 - 1.20 | Updates in this patch:<ul><li>`v2.0.7` contains a fix for vulnerability scan issues.</li><li>Updates the base image from `alpine` to `UBI`.</li><li>Pods and containers now run as `non-root` except for the `node-server` pod's containers.</li></ul> |
| `2.0.3_375` | `v2.0.6` | 17 September 2020 | 1.15 - 1.19 | Fixes an issue with volume attachment when replacing workers. |
| `2.0.3_374+` | `v2.0.5` | 29 August 2020 | 1.15 - 1.19 | Adds the `/var/lib/kubelet` path for CSI driver calls on OCP 4.4. |
| `2.0.3_365` | `v2.0.4` | 05 August 2020 | 1.15 - 1.19 | <ul><li>Updates sidecar container images.</li><li>Adds liveness probe.</li><li>Enables parallel attachment and detachment of volumes to worker nodes. Previously, worker nodes were attached and detached sequentially.</li></ul> |
{: row-headers}
{: class="comparison-table"}
{: caption="Patch updates for version 2.0.3" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the patch version number of the component. The second column contains the image tag the component. The third column contains the release date of the patch. The fourth column contains the supported cluster versions. The fifth column contains a brief description of the change made to the component."}