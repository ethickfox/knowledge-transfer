What is Helm?
• A **package manager** for Kubernetes
• Helps **define, install, upgrade, and manage** Kubernetes applications
• Uses **Helm Charts** (pre-packaged templates) to deploy applications

🔹 Why Use Helm?
✅ Simplifies Kubernetes deployment management
✅ Enables **version control** and **rollback**
✅ Reduces YAML duplication with **templating**
✅ Supports **configuration management** with values.yaml

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx  # Search for available charts
helm install my-nginx bitnami/nginx  # Install an Nginx chart
helm list
```
A Helm chart contains:
	📁 Chart.yaml – Metadata (name, version, description)
	📁 values.yaml – Default configuration values
	📁 templates/ – Kubernetes manifests with templating
	📁 charts/ – Dependencies
Deploy 
```Shell
helm install my-app ./my-chart
```
Remove
```Shell
helm uninstall my-nginx
```
Display
```Shell
helm list
```
