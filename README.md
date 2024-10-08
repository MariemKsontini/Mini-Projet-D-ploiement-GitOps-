# Mini-Projet-Deploiement-GitOps-

# Documentation du Pipeline de Déploiement GitOps

## Introduction
Ce repository contient tous les fichiers nécessaires pour déployer une application web simple (Hello server!) via un pipeline de déploiement GitOps utilisant ArgoCD sur un cluster AKS (Azure Kubernetes Service).

## Structure du Repository
- `/MyApplicationResources`: Contient tous les fichiers de configuration Kubernetes nécessaires pour le déploiement de l'application.
- `/terraform`: Contient le iac pour créer le kubernetes cluster sur Azure.
- `README.md`: Fournit la documentation sur comment configurer et utiliser le pipeline.

## Étapes de Configuration

### 1. Création et Configuration du Cluster AKS

### 2. Installation d'ArgoCD
Installez ArgoCD dans votre cluster Kubernetes. Pour l'installation, exécutez:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Configuration de l'Accès via Port Forwarding
Pour accéder à l'interface utilisateur d'ArgoCD localement, utilisez le port forwarding:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 4. Connexion à ArgoCD
Connectez-vous à l'interface utilisateur d'ArgoCD à l'adresse `https://localhost:8080` en utilisant le nom d'utilisateur `admin` et le mot de passe récupéré via la commande suivante:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode
```

### 5. Création de l'Application dans ArgoCD
Ajoutez votre repository Git à ArgoCD et configurez une nouvelle application en liant les manifests situés dans le dossier `/MyApplicationResources`.

### 6. Automatisation des Déploiements
Configurez ArgoCD pour surveiller la branche principale du repository. ArgoCD déploiera automatiquement les modifications poussées sur cette branche.

## Usage
Pour déployer une nouvelle version de l'application, mettez à jour les manifests si nécessaire, puis poussez les modifications sur la branche principale. ArgoCD détectera les changements et déclenchera un déploiement automatique.

## Conclusion
Ce pipeline GitOps permet une gestion simplifiée et automatisée des déploiements d'applications sur AKS, offrant un aperçu clair des versions déployées et facilitant les rollbacks si nécessaire.
