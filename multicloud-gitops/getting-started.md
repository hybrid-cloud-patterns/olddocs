---
layout: default
title: Getting Started
grand_parent: Patterns
parent: Multicloud GitOps
nav_order: 1
---

# Deploying the Multicloud GitOps pattern

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

## Prerequisite

* An OpenShift cluster
  * To create an OpenShift cluster, go to the [Red Hat Hybrid Cloud console](https://console.redhat.com/).
  * Select **OpenShift -> Clusters -> Create cluster**.
  * The cluster must have a dynamic `StorageClass` to provision `PersistentVolumes`. See [sizing your cluster](../../multicloud-gitops/cluster-sizing).
* Optional: A second OpenShift cluster for multicloud demonstration.
* A GitHub account and, optionally, a token for it with repositories permissions, to read from and write to your forks.
* The Helm binary. For details, see [Installing Helm](https://helm.sh/docs/intro/install/).

The use of this pattern depends on having at least one running Red Hat
OpenShift cluster. It is desirable to have a cluster for deploying the GitOps
management hub assets and a separate cluster(s) for the managed cluster(s).

If you do not have a running Red Hat OpenShift cluster, you can start one on a
public or private cloud by using [Red Hat's cloud
service](https://console.redhat.com/openshift/create).

## Procedure

1. Install the installation tooling dependencies. You will need:

   {% include prerequisite-tools.md %}

2. Fork the [multicloud-gitops](https://github.com/hybrid-cloud-patterns/multicloud-gitops) repository on GitHub. It is necessary to fork because your fork will be updated as part of the GitOps and DevOps processes.

3. Clone the forked copy of this repository.

    ```sh
    git clone git@github.com:your-username/multicloud-gitops.git
    ```

4. Create a local copy of the Helm values file that can safely include credentials.

    **Warning:**
    Do not commit this file. You do not want to push personal credentials to GitHub.

    ```sh
    cp values-secret.yaml.template ~/values-secret.yaml
    vi ~/values-secret.yaml
    ```

5. Customize the deployment for your cluster.

   ```sh
   git checkout -b my-branch
   vi values-global.yaml
   git add values-global.yaml
   git commit values-global.yaml
   git push origin my-branch
   ```

6. You can deploy the pattern using the [validated pattern operator](/infrastructure/using-validated-pattern-operator/). If you do use the Operator, then skip Verification.

## Verification

1. Preview the changes.

    ```sh
    make show
    ```

2. Login to your cluster using oc login or exporting the `KUBECONFIG`.

    ```sh
    oc login
    ```

    or set `KUBECONFIG` to the path to your `kubeconfig` file. For example:

    ```sh
    export KUBECONFIG=~/my-ocp-env/hub/auth/kubconfig
    ```

3. Apply the changes to your cluster.

    ```sh
    make install
    ```

4. Verify that the Operators have been installed.
    1. To verify, in the *OpenShift Container Platform web console, navigate to **Operators → Installed Operators** page.
    2.Check that the Operator is installed in the `openshift-operators` namespace and its status is `Succeeded`.
<!-- Get a SME review for this step 5 -->
5. Verify that all applications are synchronized. Under the project `multicloud-gitops-hub` click the URL for the `hub` gitops `server`. The Vault application is not synched.

[![Multicloud GitOps Hub](/images/multicloud-gitops/multicloud-gitops-argocd.png)](/images/multicloud-gitops/multicloud-gitops-argocd.png)

<!-- Moved Deploying the managed cluster applications section under next step (or it should be a separate file-->

## Multicloud GitOps application demos

As part of this pattern, HashiCorp Vault has been installed. Refer to the section on [Vault](https://hybrid-cloud-patterns.io/secrets/vault/).

<!--The Next steps heading is not inline with the chapter and only points to contibution links for help and feedback or bugs -->
# Next steps

## Deploying the managed cluster applications

After the management hub is set up and works correctly, attach one or more managed clusters to the architecture (see diagrams below).

For instructions on deploying the edge, refer to [Managed Cluster Sites](https://hybrid-cloud-patterns.io/multicloud-gitops/managed-cluster/).

>Contribute to this pattern:
[Help & Feedback](https://groups.google.com/g/hybrid-cloud-patterns){: .btn .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Report Bugs](https://github.com/hybrid-cloud-patterns/multicloud-gitops/issues){: .btn .btn-red .fs-5 .mb-4 .mb-md-0 .mr-2 }
