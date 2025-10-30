# 🚀 Helm + Kustomize + ArgoCD Workflow Study Guide
*A friendly illustrated guide to GitOps automation with Kubernetes*  
👩🏽‍💻 **By Rosine Mukuralinda**

---

## 🧭 The Big Picture
Helm, Kustomize, and ArgoCD are like a three-step automation chain working together inside Kubernetes:

1. 🧱 **Helm** → builds the Kubernetes templates (like a reusable recipe kit)  
2. 🧩 **Kustomize** → customizes those templates for each environment (dev/prod)  
3. 🤖 **ArgoCD** → automatically applies and syncs the changes from Git to your cluster  

### 🎯 Real-Life Analogy
> 🧁 **Helm** = cupcake recipe (same dough)  
> 🌈 **Kustomize** = frosting per environment (dev = chocolate, prod = vanilla)  
> 🤖 **ArgoCD** = robot baker that watches your recipe book (Git) and updates the kitchen (cluster) whenever you change it  

---

## 🧱 Helm — The Template Builder
Helm bundles all your YAML manifests (Deployments, Services, etc.) into a reusable *chart*.

### 📁 Folder Structure
helm-charts/
└─ charts/
└─ demo-app/
├─ Chart.yaml
├─ values.yaml
├─ dev-values.yaml
├─ prod-values.yaml
└─ templates/
├─ deployment.yaml
├─ service.yaml
└─ ingress.yaml

### ⚙️ Command Example
```bash
helm template demo-app ./demo-app -f ./demo-app/dev-values.yaml
In plain language: Helm takes your values.yaml + templates → builds one big YAML → gives it to Kubernetes.

🧩 Kustomize — The Config Customizer

Kustomize helps you avoid copying YAML files for each environment.
You define a base and then make overlays (patches) for dev/prod.
Example Structure
base/
 ├─ deployment.yaml
 ├─ service.yaml
 └─ kustomization.yaml

overlays/
 ├─ dev/
 │   └─ kustomization.yaml
 └─ prod/
     └─ kustomization.yaml

Commands
kubectl apply -k overlays/dev
kubectl apply -k overlays/prod

Analogy

Base = cupcake recipe (common YAMLs)

Overlay = frosting for each environment (adds tweaks)

Kustomize = the baker that merges both → the final cupcake 🍰 (final YAML Kubernetes actually uses)

🤖 ArgoCD — The GitOps Robot

ArgoCD keeps your cluster in sync with your Git repo.
Whenever you commit an update to Helm/Kustomize files, ArgoCD redeploys automatically.

🧩 Workflow
GitHub → ArgoCD Server → CRDs → Kubernetes Cluster → Pods/Services

⚙️ Install Steps
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl -n argocd port-forward svc/argocd-server 8080:80


➡️ Login: open http://localhost:8080

Default user: admin
Password:

kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d

💡 Lame-Language Summary

ArgoCD is your DevOps robot chef.
It watches your Git repo for recipe changes and instantly “cooks” them in Kubernetes.

🌍 Ingress + Service — The Traffic Map

Kubernetes networking can be simplified like this:

Browser → Ingress → Service (ClusterIP) → Pod (container:8080)

🔄 Port-Forward Alternative
localhost:8081 → svc/demo-app → container:8080


Ingress = permanent doorway (used for real environments)
Port-forward = temporary shortcut (used for local testing)

👩🏽‍💻 Rosine’s Lab Notes
✅ Minikube Setup
minikube start
kubectl get ns

✅ Installed Tools

🧱 Helm

🧩 Kustomize

🤖 ArgoCD

📦 kubectl

✅ Deployed Apps

demo-app (dev + prod)

ArgoCD UI running locally via port-forward

Working Ingress Controller (ingress-nginx)

✅ Clean-Up Commands
kubectl delete ns dev prod argocd ingress-nginx
minikube delete


💬 This resets your cluster for your next project.

🧩 Full Workflow Recap
Helm → builds templates
Kustomize → customizes configs
ArgoCD → syncs cluster with Git
Ingress → exposes app to browser

💬 Quick Reference
Term	Plain Meaning	Example
Helm Chart	Reusable package of YAML files	helm install demo-app ./demo-app
Kustomize	Customizes YAML per environment	kubectl apply -k overlays/dev
ArgoCD	Robot that syncs Git with cluster	auto-deploys updates
Ingress	Doorway into your app	demo.localtest.me
Service	Router inside cluster	directs traffic to Pods
Namespace	Folder for grouping objects	dev, prod, argocd
🧠 Mini Quiz

If Helm builds and Kustomize customizes, what does ArgoCD do?

Why is it better to store configs in Git instead of editing them directly in Kubernetes?

What’s the difference between service.port and containerPort?

✨ Final Thought

“GitOps is about letting automation handle deployments
so you can focus on building — not babysitting pods.” 🚀

© 2025 — Rosine Mukuralinda
DevOps Study Project at Emory Healthcare


---

### ✅ Next Steps for You
1. Open your repo in VS Code.  
2. Create a new file at the root:  
   ```bash
   touch README.md


Copy-paste everything above.

Commit and push:

git add README.md
git commit -m "Add Helm + Kustomize + ArgoCD Study Guide"
git push origin main
