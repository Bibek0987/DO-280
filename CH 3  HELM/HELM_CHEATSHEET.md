# ============================================================
# HELM COMMANDS CHEAT SHEET
# ============================================================
# Helm is the package manager for Kubernetes.
# Charts = packages | Releases = installed instances of a chart.
# ============================================================


# ------------------------------------------------------------
# 📦 CHART & REPOSITORY MANAGEMENT
# ------------------------------------------------------------

# Add a chart repository to your local Helm config
helm repo add bitnami https://charts.bitnami.com/bitnami

# List all currently configured repositories
helm repo list

# Update local cache of charts from all added repositories
helm repo update

# Remove a repository from your local config
helm repo remove bitnami

# Search for charts across added repositories
helm search repo nginx

# Search the official Helm Hub / Artifact Hub (online)
helm search hub wordpress

# Download a chart to your local machine (as .tgz)
helm pull bitnami/nginx

# Download and unpack the chart into a local directory
helm pull bitnami/nginx --untar

# Show the default values.yaml of a chart (useful before install)
helm show values bitnami/nginx

# Show the Chart.yaml metadata of a chart
helm show chart bitnami/nginx

# Show the README of a chart
helm show readme bitnami/nginx

# Scaffold a new chart directory with default structure
helm create my-chart

# Package a chart directory into a versioned .tgz archive
helm package ./my-chart

# Lint a chart to catch errors before installing
helm lint ./my-chart

# Update/download sub-chart dependencies defined in Chart.yaml
helm dependency update ./my-chart


# ------------------------------------------------------------
# 🚀 INSTALLATION & LIFECYCLE
# ------------------------------------------------------------

# Install a chart into the cluster (release name + chart reference)
helm install my-release bitnami/nginx

# Install into a specific namespace (creates it if missing)
helm install my-release bitnami/nginx --namespace my-ns --create-namespace

# Install with custom values file
helm install my-release bitnami/nginx -f custom-values.yaml

# Install with inline value overrides
helm install my-release bitnami/nginx --set replicaCount=3

# Simulate install without applying to cluster (dry run)
helm install my-release bitnami/nginx --dry-run --debug

# List all releases in the current namespace
helm list

# List all releases across all namespaces
helm list --all-namespaces

# List all releases including failed/uninstalled ones
helm list --all

# Check status of a specific release
helm status my-release

# Upgrade an existing release to a new chart version
helm upgrade my-release bitnami/nginx

# Upgrade with new values file
helm upgrade my-release bitnami/nginx -f new-values.yaml

# Upgrade, installing if the release does not exist yet
helm upgrade --install my-release bitnami/nginx

# Upgrade and automatically roll back if the upgrade fails
helm upgrade my-release bitnami/nginx --atomic

# Roll back a release to a previous revision
helm rollback my-release 1

# View revision history of a release
helm history my-release

# Uninstall (delete) a release and its Kubernetes resources
helm uninstall my-release

# Uninstall but keep the release history (for auditing)
helm uninstall my-release --keep-history

# Run test hooks defined inside a chart
helm test my-release


# ------------------------------------------------------------
# 🔍 DEBUGGING & INSPECTION
# ------------------------------------------------------------

# View the rendered Kubernetes manifests applied to the cluster
helm get manifest my-release

# View the values actually used by a deployed release
helm get values my-release

# View all values (including defaults) used by a release
helm get values my-release --all

# View the release notes / status output
helm get notes my-release

# View all resources created by a release
helm get all my-release

# Render chart templates locally (no cluster interaction)
helm template my-release bitnami/nginx

# Render templates with custom values for validation
helm template my-release bitnami/nginx -f values.yaml

# Show the computed values Helm would use (defaults + overrides)
helm show values bitnami/nginx


# ------------------------------------------------------------
# ⚙️ UTILITY & CONFIGURATION
# ------------------------------------------------------------

# Print the Helm client version
helm version

# Show Helm environment variables and paths
helm env

# List installed Helm plugins
helm plugin list

# Install a Helm plugin (example: helm-diff)
helm plugin install https://github.com/databus23/helm-diff

# Generate shell autocompletion (example: Bash)
helm completion bash > ~/.helm-completion.bash

# Generate shell autocompletion (example: Zsh)
helm completion zsh > "${fpath[1]}/_helm"

# Display help for any command
helm --help

# Display help for a specific subcommand
helm install --help
