
## GCP Managed Prometheus : Managed Collection + Grafana + Workload Identity + Terraform

This code demonstrates the use of Google Cloud managed service for parameters and managed collection to pull metrics from applications deployed in a GKE cluster. The Terraform code in [terraform/](terraform/) shows how to create a VPC and GKE cluster and deploy managed collection to the Kubernetes cluster.

Workload identity is used to grant necessary permissions to the collectors that push metrics to managed parameters. Node exporters are deployed on each node to collect metrics using Prometheus-based collectors. The deployment of node exporters can be found in [k8s/node-exporter/](k8s/node-exporter/).

Port monitoring custom resources provided by the managed collection operator are used to scrape the node exporters. The code allows you to query and create dashboards using the Google Cloud UI for monitoring and observability.

To use Grafana, a workaround is needed as it doesn't support authentication using OAuth2. A separate component called Prometheus UI is deployed as a proxy. The deployment and configuration of Prometheus UI and Grafana can be found in [k8s/grafana/](k8s/grafana/).

The deployment includes the necessary annotations and permissions to enable workload identity and access to managed parameters. The code provides instructions on how to verify the setup and troubleshoot any permission errors.

It also explains how to add additional targets for monitoring, such as your application, using port monitoring custom resources. The README highlights the benefits of using managed collection versus self-deployed collectors and integrating with the Prometheus operator.

The Terraform code includes the necessary providers and enables Google Cloud APIs for creating the GKE cluster. It sets up a private subnet with secondary ranges for Kubernetes ports and services. The code recommends manually allocating external IP addresses for the GKE nodes to set up firewalls and provide IAM permissions.

Workload identity is enabled for the GKE cluster, requiring the creation of Google Cloud service accounts and binding them with Kubernetes service accounts. The code demonstrates how to deploy node exporters using a DaemonSet and create the necessary monitoring namespace, service account, cluster role, and cluster role binding.

It explains the use of port monitoring custom resources for managing ports deployed in the same or multiple namespaces. The README provides instructions on how to apply the Terraform code and deploy the node exporters and managed collection.

Finally, it explains how to access the deployed components, including the Prometheus UI, Grafana, and Parameters UI, for monitoring and visualization.


##

1. Apply the infrastructure
terraform apply
1. Apply the infrastructure
kubectl get nodes
kubectl apply -f <file> (This command is implied when the narrator mentions deploying node exporter, managed collection, and other components. Replace <file> with the actual file path.)
kubectl get pods
kubectl delete pods <pod_name> (This command is implied when the narrator mentions deleting pods to restart them. Replace <pod_name> with the actual pod name.)
kubectl get serviceaccount <service_account_name> -o yaml (This command is implied when the narrator mentions getting the yaml of a service account. Replace <service_account_name> with the actual service account name.)
kubectl annotate serviceaccount <service_account_name> iam.gke.io/gcp-service-account=<gcp_service_account>@<project_id>.iam.gserviceaccount.com (This command is implied when the narrator mentions adding annotation to the kubernetes service account. Replace <service_account_name>, <gcp_service_account>, and <project_id> with the actual values.)
kubectl port-forward <pod_name> 1990:1990 (This command is implied when the narrator mentions using port forward to expose Prometheus UI. Replace <pod_name> with the actual pod name.)
kubectl port-forward <pod_name> 3000:3000 (This command is implied when the narrator mentions using port forward to expose Grafana. Replace <pod_name> with the actual pod name.)
