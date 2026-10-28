# Check if bitnami is already in your local Helm repo list
$$ helm repo list

# If bitnami is NOT listed, add it (this only affects your local CLI, not the cluster)
$$ helm repo add bitnami https://charts.bitnami.com/bitnami
$$ helm repo update

# Confirm the nginx chart is searchable
$$ helm search repo bitnami/nginx

# Create independent namespaces for DEV and PROD
$$ oc new-project nginx-dev
$$ oc new-project nginx-prod

DEV Environment Configuration :--->> 
Create nginx-dev-values.yaml:
# nginx-dev-values.yaml
replicaCount: 2
resources:
  requests:
    cpu: 10m
    memory: 70Mi
  # DEV does not set limits, inheriting chart defaults (usually empty)

Install to DEV:
# Install in DEV namespace, referencing only the DEV values file
$$ helm install nginx-dev bitnami/nginx -n nginx-dev -f nginx-dev-values.yaml

PROD Environment Configuration :---->>
Create nginx-prod-values.yaml:
# nginx-prod-values.yaml
replicaCount: 3
resources:
  requests:
    cpu: 20m
    memory: 80Mi
  limits:
    cpu: 30m
    memory: 120Mi

Install to PROD:
# Install in PROD namespace, referencing only the PROD values file
$$ helm install nginx-prod bitnami/nginx -n nginx-prod -f nginx-prod-values.yaml

Verification
bash
# Verify DEV configuration
$$ helm get values nginx-dev -n nginx-dev
$$ oc get pods -n nginx-dev
# Expected: 2 Pods, resources.requests.cpu=10m, memory=70Mi

# Verify PROD configuration
$$ helm get values nginx-prod -n nginx-prod
$$ oc get pods -n nginx-prod
# Expected: 3 Pods, requests.cpu=20m/memory=80Mi, limits.cpu=30m/memory=120Mi
