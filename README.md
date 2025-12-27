**Jeevant’s Kubernetes GitOps Home Lab**
**Overview**
This repository documents my Kubernetes GitOps Home Lab — a production-style, self-managed Kubernetes cluster built using GitOps principles.
**The cluster consists of two nodes:**
1 Control Plane (Master) node
2 Raspberry Pi 5 Worker node

**It hosts multiple self-managed applications such as Audiobookshelf, Mealie, and Linkding, and integrates FluxCD, Renovate, SOPS + age, and a full observability stack with Prometheus, Grafana, Loki, and Alertmanager.**

**The entire platform is fully declarative, version-controlled, and self-healing, with no manual kubectl apply workflows.**

**Kubernetes Cluster Details**
Platform: Self-managed Kubernetes (Home Lab)
Distribution: Kubernetes / kubeadm-style setup
Cluster Size: 2 nodes

**Node Architecture**
Role	Node Type	Purpose
Control Plane	x86 Master Node	API Server, Scheduler, Controller Manager, etcd
Worker	Raspberry Pi 5	Application workloads and services
Both nodes use static IP addresses (Ethernet) for stability
Designed to simulate real-world multi-node production environments
Nodes can be safely rebooted independently without impacting cluster state

**Key Capabilities**
This home lab demonstrates:
End-to-end GitOps deployments using FluxCD
Automated image and Helm chart updates via Renovate Bot

**Secure secret management using SOPS + age**
Centralized monitoring and logging with Prometheus, Grafana, Loki, Alertmanager
Secure external access via Cloudflare Tunnel (no public node exposure)
Zero manual operations — Git is the single source of truth

**High-Level Architecture**
GitHub Repository
        ↓
     FluxCD
        ↓
Kubernetes Cluster
 ├─ Control Plane (x86)
 └─ Raspberry Pi 5 Worker
        ↓
 Applications & Monitoring

**Namespaces**
Namespace	Purpose
flux-system	FluxCD controllers
cloudflared	Cloudflare Tunnel connector
media	Audiobookshelf
bookmarks	Linkding
mealie	Recipe management
monitoring	Prometheus, Grafana, Loki, Alertmanager

**FluxCD (GitOps Engine)**
FluxCD continuously reconciles the Kubernetes cluster state with the Git repository.
Any change merged into Git — manual or Renovate-generated — is automatically applied.

**Useful Commands**
flux reconcile kustomization flux-system --with-source
flux get kustomizations
flux get helmreleases

**GitOps Guarantees**
✅ Declarative infrastructure
✅ Drift detection & auto-healing
✅ Environment parity
✅ Full auditability via Git history

**Renovate Bot (Automated Updates)**
Renovate automatically:
Scans Helm charts and Docker images
Creates upgrade pull requests
Triggers Flux reconciliation upon merge

**Example Renovate PR**
⬆️ Bump Mealie image from v1.9.0 → v1.9.1

**Secret Management (SOPS + age)**
All Kubernetes secrets are encrypted before committing to Git.
Encryption: age
Decryption: Handled automatically by Flux controllers
sops --encrypt --age <AGE_PUBLIC_KEY> secrets.yaml > secrets.enc.yaml
Secrets are never stored in plaintext.

**Applications Deployed**
Application	URL	Description
**Audiobookshelf**	audiobooks.jeevant.org	Audiobook & podcast server
**Linkding**	ldhlab.jeevant.org	Bookmark manager
**Mealie**	mealie.jeevant.org	Recipe management
**Grafana**	grs.jeevant.org	Cluster monitoring & dashboards

**All applications are securely exposed using Cloudflare Tunnel — no NodePort, LoadBalancer, or public IPs.**

**Observability Stack**
Component	Purpose
Prometheus	Metrics collection
Grafana	Dashboards & visualization
Loki	Centralized logs
Alertmanager	Alert routing

**Provides real-time visibility into both x86 and Raspberry Pi workloads.**

**Deployment Flow**
Change committed to GitHub
Flux reconciles the cluster
Secrets decrypted via SOPS + age
Applications deployed or updated automatically
Metrics → Prometheus
Dashboards → Grafana
Logs → Loki
Alerts → Alertmanager

**Repository Structure**
home-cluster/
├── apps/
│   ├── base/
│   └── staging/
├── clusters/
│   └── staging/
│       └── flux-system/
└── README.md

**Future Enhancements**
Terraform-based infrastructure automation
Advanced Cloudflare Zero Trust rules
Multi-architecture image optimization (ARM + x86)
ArgoCD comparison study

**Author**
Jeevant Kumar
**☁️ DevOps | SRE | Kubernetes (CKA) | GitOps | Cloud | Automation**
**🔗 LinkedIn**
https://www.linkedin.com/in/jeevant-kumar-80b55826/
