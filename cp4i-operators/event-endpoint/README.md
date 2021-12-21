# Event end point management deployment

Deploy the end point management as an API Connect capability.
Depending of the deployed operator the steps may bee different.

* Install the API Connect operator

  ```sh
  oc apply -k c4pi-operators/apic-connect/operator/overlays
  ```
* Get a storage class with gid and modify the manifest: `c4pi-operators/event-endpoint/eventendpointmanager-eepm-eda.yaml`

  ```yaml
  storageClassName: cp4a-file-retain-gold-gid
  ```

* Install the end point service

  ```sh
  oc apply -k c4pi-operators/event-endpoint/
  ```

This could take some time. To monitor how the deployment executes, go to the `openshift-operators` and the `apic-connect` pod.