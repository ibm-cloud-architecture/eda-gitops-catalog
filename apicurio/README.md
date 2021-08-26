# Apicurio Registry Operator and instance 

Install Apicurio Registry operator with monitoring all namespaces.

Current Channel includes `2.x`.

## Usage

### Operator

If you have cloned the `eda-gitops-catalog` repository, you can install the Apicurio Registry operator

```sh
oc apply -k apicurio/operator
```

Or, without cloning:

```sh
oc apply -k https://github.com/redhat-cop/gitops-catalog/amq-streams-operator/overlays/<channel>
```

As part of a different overlay in your own GitOps repo:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
  - github.com/jbcodeforce/eda-gitops-catalog/kafka-strimzi/operator?ref=main
```

### Kafka Cluster instance

This folder also includes a base cluster for development purpose. Be sure to be in the target
project to deploy your cluster.

```sh
# for example
oc new-project kafka-strimzi-25
oc apply -k kafka-strimzi/instance/
```
