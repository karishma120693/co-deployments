## HPE COSI Driver for Kubernetes Helm Chart

The HPE COSI Driver for Kubernetes leverages Hewlett Packard Enterprise object storage platforms to provide scalable S3 compatible object storage for workloads running on Kubernetes. Currently supported object storage platforms include HPE Alletra Object Placeholder.

## Prerequisites

- Kubernetes v1.30 or later
- Helm v3.11 or later
- HPE Alletra Object Placeholder
  - S3 backend protocol
  - Features available to the COSI driver: Bucket creation and deletion, granting and revoking bucket access

## Configuration and Installation

The following parameters are supported by the Helm chart. During normal circumstances, these can be left at their default values.

| Parameter                 | Description           | Default
| :------------------------ | :-------------------- | :-------------
| imagePullPolicy           | Image pull policy of the images. `Always`, `IfNotPresent` or `Never` | `IfNotPresent` |
| images.cosiDriver         | Fully qualified registry path to COSI driver image. | from values.yaml |
| images.cosiSideCar        | Fully qualified registry path to COSI driver sidecar image. | from values.yaml |
| sideCarVerbosityLevel     | Verbosity level of the logs printed by the sidecar container. | 5 |
| resources.limits.cpu      | CPU limits applied to the COSI driver and the COSI sidecar individually. | Not set |
| resources.limits.memory   | Memory limits applied to COSI driver and COSI the sidecar individually. | Not set |
| resources.requests.cpu    | CPU requests applied to the COSI driver and COSI the sidecar individually. | Not set |
| resources.requests.memory | Memory requests applied to the COSI driver and COSI the sidecar individually. | Not set |

Learn how to specify resource limits and requests in the official documentation on [kubernetes.io](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

	## Installation Steps

1. Create the custom resource definitions (CRDs) for the COSI driver API resources:

		```console
		kubectl apply -k github.com/kubernetes-sigs/container-object-storage-interface-api
		```

2. Deploy the object storage controller:

		```console
		kubectl apply -k github.com/kubernetes-sigs/container-object-storage-interface-controller
		```

4. Install the Helm chart:

		```console
		helm install --create-namespace -n hpe-object my-hpe-cosi-driver .

	NAME: my-hpe-cosi-driver
    LAST DEPLOYED: Thu Aug 22 16:41:32 2024
    NAMESPACE: hpe-object
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
		```

		Verify the driver is running:

		```console
		kubectl get deploy -n hpe-object
	NAME              READY   UP-TO-DATE   AVAILABLE   AGE
    hpe-cosi-driver   1/1     1            1           8s
	```


	## Using

	Refer to the documentation in the GitHub repo for examples on how to create object storage resources.
