# Helm Chart for DeepWiki-Open

This Helm chart is designed to deploy the [DeepWiki-Open](https://github.com/AsyncFuncAI/deepwiki-open) project within a Kubernetes cluster. It provides deployment templates, ingress configuration, and service definitions tailored for the DeepWiki-Open application, with customization options via `values.yaml`.

## Key Features
- Deploy a multi-container application with persistent storage
- Configure ingress for both web and API endpoints
- Customize environment variables and resource limits
- Use a pre-configured image from GitHub Container Registry

## Installation

1. Add the chart repository (if using a custom repo):
<pre><code>
helm repo add mychart https://your-chart-repo.com
</code></pre>

2. Install the chart with custom values:
```
helm install my-release ./chart --set spec.namespace=your-namespace \
  --set spec.replicas=2 \
  --set ingress.url=your-web-url
```

## Configuration

The following parameters can be customized in `values.yaml`:

### Core Settings
- `spec.namespace`: Kubernetes namespace (default: `deepwiki`)
- `spec.replicas`: Number of application replicas (default: `1`)
- `spec.image`: Container image (default: `ghcr.io/asyncfuncai/deepwiki-open:sha-ff354c9`)

### Environment Variables
- `env.OLLAMA_HOST`: URL for Ollama server (default: `http://your-ollama-server:11434`)

### Storage
- `storage.pvc`: Persistent Volume Claim name (default: `deepwiki-pvc`)

### Ingress
- `ingress.url`: Combined web and API domain (default: `deepwiki.example.com`)

## Template Structure

### Deployments
The `templates/deployments.yaml` file defines:
- A Deployment for the main application
- Persistent volume configuration for `/root/.adalflow`
- Custom environment variables from `values.yaml`

### Services
The `templates/services.yml` file defines:
- A service for the web interface (port 3000)
- A service for the API endpoint (port 8001)

### Ingress
The `templates/ingress.yml` file configures:
- Separate ingress rules for web and API endpoints
- HTTP backend protocol with proxy settings
- Custom domain routing for both endpoints

## Dependencies
This chart has no external dependencies beyond standard Kubernetes resources. However, the following prerequisites must be in place:
- A persistent volume and claim named `deepwiki-pvc`
- A valid DNS entry for the configured ingress domains

## Testing
To verify dependencies and configuration:

```
helm dependency update
```

## Contribution
We welcome improvements! Please:
1. Add new configuration options
2. Update documentation
3. Fix any issues you encounter

For help, check the [Helm documentation](https://helm.sh/docs/)
