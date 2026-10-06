# homelab-gitops

Desired state for my Talos Kubernetes homelab, reconciled by Argo CD.

Clusters are built by Terraform in
[homelab-terraform](https://github.com/MrMimeDanceTime/homelab-terraform):
Talos bootstraps Cilium and Argo CD, then Argo CD takes over and manages
everything here, itself included.

Nothing secret lives in this repository. Secrets are pulled at runtime from
Infisical by External Secrets Operator.

## Layout

| Path | What |
|---|---|
| `clusters/k8s-dev/` | Applications for the k8s-dev cluster. Argo CD syncs every file here. |
| `clusters/k8s-dev/root.yaml` | The app-of-apps itself |
| `clusters/k8s-dev/argocd.yaml` | Argo CD, managing its own upgrades |

## Bootstrap

Talos applies two inline manifests when a cluster is built: Cilium (pod
networking) and Argo CD plus `root`. Talos applies each object once and never
again, so after the first sync Argo CD owns everything, including itself.

## Access

Until a gateway exists:

```bash
kubectl -n argocd port-forward svc/argocd-server 8080:80
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```
