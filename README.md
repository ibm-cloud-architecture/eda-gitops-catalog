# Event-driven Solution gitOps catalog

The EDA GitOps Catalog includes kustomize bases and overlays for a number of IBM Cloud Pak for Integration OpenShift operators needed
to develop event-driven solutions.

This repository is using the same structure as introduced by Red Hat COP team in [this repository](https://github.com/redhat-cop/gitops-catalog).

!!! info
    Updated 1/7/2022: Event Streams 2.5.1

This catalog includes definition for operators, and some example of operands. But the real appraoch
is to use this catalog from another, project or solution based gitops repository, as illustrated
with the following GitOps repository:

* []()

## Usage

Each catalog item has its own README.md for future instructions. Be sure to use the most recent `oc` CLI.

See the download page [here](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/stable/).

```sh
git clone https://github.com/ibm-cloud-architecture/eda-gitops-catalog.git
```

Be sure to be connected to an OpenShift Cluster.

Then apply one of the defined operator and if you want to test you can try some sample operands too.

### GitOps

If you are using GitOps as a way to deploy products and services you need to deploy the GitOps and Pipelines Operators.

```sh
oc apply -k openshift-gitops/operator/overlays
oc apply -k openshift-pipelines/operator/overlays
```

### IBM Catalog

To install IBM products you need to deploy the catalog definition to OpenShift.

```sh
# List existing catalog
oc get catalogsource -n openshift-marketplace
# If the IBM catalogs are not displayed add the following:
oc apply -k ibm-catalog -n openshift-marketplace
# With this catalog we should be able to search for IBM products
oc get packagemanifests -n openshift-marketplace| grep ibm | sort
# Inspect MQ operator
oc describe packagemanifests ibm-mq -n openshift-marketplace
```

### Define secret for IBM products license

* Obtain your [IBM license entitlement key](https://github.com/IBM/cloudpak-gitops/blob/main/docs/install.md#obtain-an-entitlement-key)
* Create the [OCP global pull secret of the `openshift-gitops` project](https://github.com/IBM/cloudpak-gitops/blob/main/docs/install.md#update-the-ocp-global-pull-secret)
with the entitlement key

    ```sh
    KEY=<yourentitlementkey>
    oc create secret docker-registry ibm-entitlement-key \
    --docker-username=cp \
    --docker-password=$KEY \
    --docker-server=cp.icr.io \
    --namespace=openshift-gitops 
    ```

### Cloud Pack for Integration 

* Install Platform Navigator operator to get access to the integrated user interface:

```sh
oc apply -k cp4i-operators/platform-navigator/operator/overlays
```

* Sample Operands: to create on instance of the platform navigator

```sh
oc apply -k cp4i-operators/platform-navigator/operands/
```

License Service is not deployed by default. The Platform Navigator sample file [cp4i-sample.yaml](./cp4i-operators/platform-navigator/operands/cp4i-sample.yaml) 
includes the fields to include it as part of the deployment. 
If you do not want to deploy it simply remove the following lines:

```yaml
  requestIbmServices:
    licensing: true
```

If as part of License Service you want to visualize the reports via the Web UI you will need to deploy an instance of the License Reporter, 
the sample file [license-reporter.yaml](./cp4i-operators/platform-navigator/operands/license-reporter.yaml) shows how to do it. 
Additionally you need to update the common-service instance of the OperandConfig operand in the Operand Deployment Lifecycle Manager operator. 
You have to include the following line in the spec section of the ibm-licensing-operator. For more details check the [Knowledge Center](https://www.ibm.com/docs/en/cpfs?topic=reporter-deploying-license-service#lrcons).

  ```yaml
  IBMLicenseServiceReporter: {}
  ```

If you want to enable monitoring you can deploy the ConfigMap included in 
file [monitoring-cm.yaml](./cp4i-operators/platform-navigator/operands/monitoring-cm.yaml) otherwise you can skip it.

* If you want to use those definitions inside of a GitOps repository see the following
examples:

  * The [GitOps](https://github.com/jbcodeforce/eda-demo-order-gitops) of a simple order management demo to illustrate developer experience to develop event-driven solution.
  * The new [GitOps]() for the Reefer Container solution - EDA reference implementation

### Install Event Streams Using CLIs

* Deploy Event Streams operator

  ```sh
  oc apply -k cp4i-operators/event-streams/operator/overlays
  ```

* Install one Event Streams operands: Instances of Event Streams can be created after the Event Streams operator is up and running. 
To verify it is running:

```
oc get pods -n openshift-operators
```

You can use the OpenShift console or our predefined cluster definition:

  ```shell
  oc apply -k cp4i-operators/event-streams/operands/dev/
  ```

  If you want to do the same thing for a production cluster

  ```shell
  oc apply -k cp4i-operators/event-streams/operands/prod-small/
  ```

### Install MQ broker

* Install MQ Operator

 ```sh
 oc apply -k cp4i-operators/mq/operator/overlays
 ```

### Install Event End Point management


* Deploy API Connect Operator

  ```sh
  oc apply -k cp4i-operators/apic-connect/operator/overlays
  ```

* From IBM API Connect operator add an Event Endpoint Manager instance

  ```sh
  oc apply -k c4pi-operators/event-endpoint/operands
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

## Contributors

* [Joel Gomez Ramirez](https://www.linkedin.com/in/jgomezr/)
* [Jesus Almaraz](https://www.linkedin.com/in/jesus-almaraz-hernandez/)
* [Jerome Boyer](https://www.linkedin.com/in/jeromeboyer/)