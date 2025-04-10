🚀 FluxCD GitOps Workflow with GKE
This repository demonstrates how to use FluxCD for Continuous Deployment (CD) to a GKE cluster. Unlike traditional CI/CD setups where Jenkins or other tools are used to deploy using kubectl apply, FluxCD enables automated GitOps-style deployments.

📌 What is FluxCD?
FluxCD is a GitOps-based Continuous Delivery tool for Kubernetes. It continuously monitors your Git repository and automatically applies changes to your cluster. It is similar to ArgoCD, but:

More CLI-based (via flux CLI)

Offers more granular control

Has a strong ecosystem of modular controllers (source-controller, kustomize-controller, image-reflector-controller, etc.)

Before GitOps tools like FluxCD or ArgoCD, teams typically used Jenkins pipelines with a manual or scripted kubectl apply process. This often required updating Docker image tags manually or using additional Jenkins stages. With FluxCD, all of that becomes automated.

🧰 Requirements
A Kubernetes Cluster (like GKE)

kubectl configured

Flux CLI installed

A GitHub repo to store your Kubernetes manifests

🧪 What Happens When You Use FluxCD?
You install Flux CLI (needed to interact with Flux).

You bootstrap Flux into your cluster using a GitHub repo.

Flux creates a flux-system namespace and installs its controllers inside it.

It also generates a folder structure and YAML files that define:

Source Git repository (GitRepository)

Sync interval and path (Kustomization)

Controller configurations

These YAMLs are pushed to your GitHub repo in a folder called flux-system.

After bootstrap:

Any manifest you commit to a folder defined in the Kustomization (e.g., clusters/prod) will automatically get applied to your cluster.

🔁 Sample flux bootstrap Command
bash
Copy
Edit
flux bootstrap github \
  --owner=mrakbg \
  --repository=manifestsforrestart \
  --branch=main \
  --path=clusters/prod \
  --personal
📖 Explanation:
Parameter	Description
--owner	GitHub username or org
--repository	Git repo name
--branch	Branch to use (usually main)
--path	The folder in the repo where manifests reside
--personal	Indicates the repo is under a personal GitHub account
This command installs all required controllers and configures Flux to sync your manifests under clusters/prod to the cluster.

📁 Folder Structure After Bootstrap
bash
Copy
Edit
manifestsforrestart/
├── clusters
│   └── prod
│       ├── kustomization.yaml  # Your kustomization logic
│       └── deployment.yaml     # Your application manifests
└── flux-system
    ├── gotk-components.yaml    # Flux controllers
    └── kustomization.yaml      # Flux configuration
🔍 Notes
Flux will only apply manifests in paths specified in the Kustomization object.

If you change replica count, image tags, or any other manifest values in Git, Flux will detect and apply them automatically.

You can also configure image automation to deploy new images automatically once pushed to a registry.

Continue.....................
