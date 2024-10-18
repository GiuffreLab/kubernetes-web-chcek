# Web Check Kubernetes Deployment

This project contains the Kubernetes manifests needed to deploy the **Web Check** application using a Deployment, Service, Namespace, and Customization configuration. The `web-check` container is based on the `lissy93/web-check` Docker image, which provides web status monitoring capabilities.

## Prerequisites

- Kubernetes cluster set up and running.
- `kubectl` configured to interact with your cluster.
- Docker image `lissy93/web-check` available.

## Files Included

- **namespace.yaml**: Defines the namespace for the `web-check` application.
- **deployment.yaml**: Describes the deployment configuration for the `web-check` container.
- **service.yaml**: Creates a service to expose the application.
- **kustomization.yaml**: Defines the customization configuration for deploying the `web-check` application.

## Deployment Steps

1. **Deploy the Application with Kustomize**

   Use the `kustomization.yaml` file to deploy the `web-check` container to the cluster:
   ```sh
   kubectl apply -k .
   ```

## Accessing the Application

- If you do not modify the `service.yaml` file, you can access the application at `localhost:30006`

- If you're using **port-forwarding**, you can access the application at `localhost:3000` by running:
  ```sh
  kubectl port-forward service/web-check-service 3000:3000 -n web-check
  ```

- If you're running in a cloud environment or using `NodePort`, adjust your cluster settings accordingly to expose the application externally.

## Security Contexts

The `deployment.yaml` file includes a **securityContext** configuration to improve container security:

- **`allowPrivilegeEscalation: false`**: Prevents privilege escalation.
- **`privileged: false`**: Ensures the container is not running in privileged mode.
- **`runAsNonRoot: true`** and **`runAsUser: 1000`**: Ensures the container runs as a non-root user.
- **`capabilities.drop: - ALL`**: Drops unnecessary Linux capabilities.

These settings help mitigate common container vulnerabilities and limit the impact of any potential security incidents.

## Clean Up

To remove all the deployed resources:
```sh
kubectl delete -k .
```

