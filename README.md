# homelab-gitops

Desired state for my Talos Kubernetes homelab, reconciled by Argo CD.

Clusters are built by Terraform in
[homelab-terraform](https://github.com/MrMimeDanceTime/homelab-terraform):
Talos bootstraps Cilium and Argo CD, then Argo CD takes over and manages
everything here, itself included.

Nothing secret lives in this repository. Secrets are pulled at runtime from
Infisical by External Secrets Operator.
