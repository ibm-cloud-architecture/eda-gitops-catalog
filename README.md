# Event-driven Solution gitOps catalog

The GitOps Catalog includes kustomize bases and overlays for a number of OpenShift operators needed
to develop event-driven solution and services.

This is using the same structure as introduce by Red Hat COP team in [this repository](https://github.com/redhat-cop/gitops-catalog).

## Usage

Each catalog item has its own README.md for future instructions. Be sure to use the most recent `oc` CLI.

See the download page [here](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/stable/).

```sh
git clone https://github.com/ibm-cloud-architecture/eda-gitops-catalog.git
```

Then apply on one of the defined product or operator.

### IBM Catalog

```sh
# List existing catalog
oc get catalogsource -n openshift-marketplace
# If the IBM catalogs are not displayed add the following:
oc apply -k ibm-catalog -n openshift-marketplace
# With this catalog we should be able to install MQ operator
oc get packagemanifests -n openshift-marketplace  
# Inspect MQ operator
oc describe packagemanifests ibm-mq -n openshift-marketplace
```

### Install Event Streams Using CLIs

This section is an alternate of using OpenShift Console. We are using our GitOps catalog repository which
defines the different operators and scripts we can use to install Event Streams and other services
via scripts. All can be automatized with ArgoCD / OpenShift GitOps.

* Create a project to host Event Streams cluster:

  ```shell
  oc new-project eventstreams
  ```
* Clone our eda-gitops-catalog project

  ```sh
  git clone https://github.com/ibm-cloud-architecture/eda-gitops-catalog.git
  ```
* Create the `ibm-entitlement-key` secret in this project.

  ```sh
  oc create secret docker-registry ibm-entitlement-key \
        --docker-username=cp \
        --docker-server=cp.icr.io \
        --namespace=eventstreams \
        --docker-password=your_entitlement_key 
  ```

* Install Event Streams Operator subscriptions

  ```sh
  oc apply -k cp4i-operators/event-streams/operator/overlays/v2.4/
  ```

* Install one Event Streams instance: Instances of Event Streams can be created after the Event Streams operator is installed. 
You can use te OpenShift console or our predefined cluster definition:

  ```shell
  oc apply -k cp4i-operators/event-streams/instances/dev/
  ```

  If you want to do the same thing for a production cluster

  ```shell
  oc apply -k cp4i-operators/event-streams/instances/prod-small/
  ```

### Install MQ broker

### Install Event End Point management


* Create a namespace
* Copy the entitlement key
* From IBM API Connect operator add an Event Endpoint Manager instance

  ```sh
  # In eda-gitop-catalog
  oc apply -k c4pi-operators/event-endpoint
  ```

## Kustomize

You can reference bases for the various tools here in your own kustomize overlay without 
explicitly cloning this repo, for example:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: product-catalog-cicd

resources:
- github.com/ibm-cloud-architecture/eda-gitops-catalog/kafka-strimzi/operator/base/?ref=main
```

This enables you to patch these resources for your specific environments. 
Note that none of these bases specify a namespace, in your kustomization overlay 
you can include the specific namespace you want to install the tool into.
