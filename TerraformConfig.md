# Local Redis with Terraform

This configuration runs Redis in a local Docker container. It does not use Kubernetes or create cloud resources. Install Terraform and Docker Desktop, and make sure Docker Desktop is running.

## Files

- `versions.tf` - Terraform and Docker provider version requirements
- `providers.tf` - Docker provider configuration
- `main.tf` - Redis image and container

## Usage

1. Start Docker Desktop and wait until its engine is running.
2. Run these commands from this folder:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```
3. Redis is available at `127.0.0.1:6379`. Remove the container when done:
   ```bash
   terraform destroy
   ```

## Terraform providers

Terraform providers are plugins that let Terraform communicate with a platform or service. A provider defines the resources and data sources Terraform can manage; it does not itself create resources until you declare them in configuration and apply a plan. A single Terraform configuration can use multiple providers.

| Provider | What it manages | Example use |
| --- | --- | --- |
| Docker (`kreuzwerker/docker`) | Local Docker images and containers | Run this Redis example without Kubernetes or a cloud account |
| Azure (`hashicorp/azurerm`) | Azure infrastructure such as resource groups, networks, and virtual machines | Provision resources in an Azure subscription |
| Kubernetes (`hashicorp/kubernetes`) | Kubernetes objects such as Deployments, Services, and ConfigMaps | Deploy applications into an existing Kubernetes cluster |
| Datadog (`DataDog/datadog`) | Monitoring configuration such as dashboards, monitors, and integrations | Manage alerts and observability configuration |
| PagerDuty (`PagerDuty/pagerduty`) | Incident-management configuration such as services, schedules, and escalation policies | Route and escalate operational incidents |
| AWS (`hashicorp/aws`) | AWS infrastructure such as S3, EC2, and databases | Provision resources in an AWS account |
| Google Cloud (`hashicorp/google`) | Google Cloud infrastructure such as Compute Engine and Cloud Storage | Provision resources in a Google Cloud project |
| GitHub (`integrations/github`) | GitHub repositories, teams, and related settings | Manage repository configuration as code |

For example, the Kubernetes provider manages objects in a cluster that already exists; it does not start Docker Desktop Kubernetes or create the cluster. Similarly, Azure, Datadog, and PagerDuty providers need the corresponding service and credentials configured. Keep credentials out of `.tf` and `.tfvars` files; use environment variables or your organization's approved secrets manager.

This example only configures the Docker provider. The providers above are alternatives or additions for other use cases, not prerequisites for running local Redis.
