---
layout: default
title: DevSecOps
nav_order: 4
has_children: true
---

# 🔐 DevSecOps & CI/CD Sécurisé

{: .fs-7 }

Intégration de la sécurité dans les pipelines de développement : SAST, DAST, SCA, container scanning, secrets management, policy as code.

---

## 🎯 Compétences développées

- **CI/CD sécurisé** : Intégration de scans dans GitHub Actions, GitLab CI
- **SAST** : Analyse statique de code (Semgrep, Bandit, SonarQube)
- **DAST** : Tests dynamiques de sécurité applicative
- **SCA** : Scan de dépendances et détection de CVE
- **Container Security** : Scan d'images Docker, signing
- **Policy as Code** : OPA, Kyverno pour Kubernetes
- **GitOps sécurisé** : ArgoCD, Flux avec contrôles de sécurité

---

## 📚 Labs de cette catégorie

| #      | Titre                     | Outils                | Durée | Difficulté | Statut     |
| ------ | ------------------------- | --------------------- | ----- | ---------- | ---------- |
| **21** | Pipeline CI/CD Sécurisé   | GitHub Actions        | 4h    | ⭐⭐⭐     | 📝 À venir |
| **22** | SAST avec Semgrep         | Semgrep, Bandit       | 3h    | ⭐⭐       | 📝 À venir |
| **23** | SCA - Scan de Dépendances | Snyk, Dependabot      | 3h    | ⭐⭐       | 📝 À venir |
| **24** | Container Image Scanning  | Trivy, Clair          | 3h    | ⭐⭐⭐     | 📝 À venir |
| **25** | Secrets Scanning dans Git | GitGuardian, Gitleaks | 2h    | ⭐⭐       | 📝 À venir |
| **26** | IaC Scanning              | Checkov, tfsec        | 3h    | ⭐⭐⭐     | 📝 À venir |
| **27** | Signing & Verification    | Cosign, Notary        | 4h    | ⭐⭐⭐⭐   | 📝 À venir |
| **28** | Policy as Code            | OPA, Kyverno          | 4h    | ⭐⭐⭐⭐   | 📝 À venir |
| **29** | GitOps Sécurisé           | ArgoCD, Flux          | 5h    | ⭐⭐⭐⭐   | 📝 À venir |
| **30** | Observabilité Sécurité    | Prometheus, Grafana   | 4h    | ⭐⭐⭐     | 📝 À venir |

---

## 🛠️ Outils utilisés

### CI/CD Platforms

- **GitHub Actions** : Workflows automatisés
- **GitLab CI** : Pipelines intégrés
- **Jenkins** : Automation server

### SAST (Static Analysis)

- **Semgrep** : Multi-language code analysis
- **Bandit** : Python security linter
- **SonarQube** : Code quality & security

### SCA (Software Composition Analysis)

- **Snyk** : Dependency scanning
- **Dependabot** : GitHub native
- **OWASP Dependency-Check**

### Container Security

- **Trivy** : Comprehensive scanner
- **Clair** : Vulnerability static analysis
- **Aqua Security** : Runtime protection

### IaC Security

- **Checkov** : Terraform/CloudFormation scanner
- **tfsec** : Terraform security scanner
- **Terrascan** : Multi-IaC tool

### Policy & Compliance

- **Open Policy Agent (OPA)** : Policy engine
- **Kyverno** : Kubernetes native policies
- **Falco** : Runtime security monitoring

---

## 📖 Ressources complémentaires

- [OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/)
- [CNCF Security TAG](https://github.com/cncf/tag-security)
- [GitHub Security Lab](https://securitylab.github.com/)
- [Snyk Learn](https://learn.snyk.io/)

---

## 🎓 Certifications visées

- 🚧 **Certified DevSecOps Professional (CDP)**
- 🚧 **Certified Kubernetes Security Specialist (CKS)**
- ✅ **Introduction to DevOps** (Coursera)

---

## 💼 Cas d'usage professionnels

Ces labs préparent aux missions d'un **DevSecOps Engineer** :

- ✅ Intégration de sécurité dans les pipelines CI/CD
- ✅ Automation des tests de sécurité
- ✅ Gestion des vulnérabilités applicatives
- ✅ Compliance as Code (SOC2, PCI-DSS)
- ✅ Collaboration Dev + Ops + Sec

---

[← Retour à l'accueil]({{ site.baseurl }}/){: .btn .btn-outline }
