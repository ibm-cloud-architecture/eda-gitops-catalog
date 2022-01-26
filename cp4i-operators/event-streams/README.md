# IBM Event Streams Operator

See product documentation for details.

Do not use the `base` directory directly, as you will need to patch the `channel` and `version` based on the version of 
OpenShift you are using, or the version of the operator you want to use.

The current *overlays* available are for the following channels:
* [2.3](overlays/v2.3)
* [2.4 - 2021.3 release](overlays/v2.4)
* [2.5 - 2021.4 release](overlays/v2.5)

## Usage

If you have cloned the `eda-gitops-catalog` repository, you can install the operator based on the overlay of your choice by running 
from the root directory

```sh
# be sure to have created `cp4i` project
oc apply -k cp4i-operators/event-streams/operator/overlays/<channel>
```

Or, without cloning:

```sh
oc apply -k https://github.com/ibm-cloud-architecture/eda-gitops-catalog/cp4i-operators/event-streams/operator/overlays/<channel>
```

As part of a different overlay in your own GitOps repo:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
  - github.com/ibm-cloud-architecture/eda-gitops-catalog/cp4i-operators/event-streams/operator/overlays/<channel>?ref=main
```