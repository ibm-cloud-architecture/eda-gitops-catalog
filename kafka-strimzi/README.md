# Strimzi Operator and Kafka Cluster Base instance

Install Strimzi operator with monitoring all namespaces.

Current Channel includes `strimzi-0.25.x`.

## Usage

### Operator

If you have cloned the `eda-gitops-catalog` repository, you can install the Strimzi operator

```sh
oc apply -k kafka-strimzi/operator/overlay/stable
```

Or, without cloning:

```sh
oc apply -k https://github.com/jbcodeforce/eda-gitops-catalog/kafka-strimzi/operator/overlay/stable
```

As part of a different overlay in your own GitOps repo:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
  - github.com/jbcodeforce/eda-gitops-catalog/kafka-strimzi/operator/overlay/stable?ref=main
```

### Kafka Cluster instance

This folder also includes a base cluster for development purpose. Be sure to be in the target
project to deploy your cluster.

```sh
# for example
oc new-project kafka-strimzi-25
oc apply -k kafka-strimzi/instance/
```
