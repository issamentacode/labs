---
layout: default
title: SOC & Détection
nav_order: 2
has_children: true
---

# 🔍 SOC & Détection d'Intrusions

{: .fs-7 }

Labs pratiques simulant le quotidien d'un **SOC Analyst** : détection de menaces, analyse de logs, corrélation d'événements, réponse aux incidents et threat hunting proactif.

---

## 🎯 Compétences développées

- **Configuration SIEM** : Installation et tuning de Wazuh, ELK Stack, Splunk
- **Règles de détection** : Création de règles custom pour détecter les menaces
- **Analyse de logs** : Parsing et corrélation de logs multi-sources
- **Use cases d'incidents** : Simulation d'attaques et investigation
- **Playbooks de réponse** : Documentation des procédures d'intervention
- **Threat Hunting** : Recherche proactive d'IOCs et de comportements suspects

---

## 📚 Labs de cette catégorie

| #      | Titre                              | Technologies             | Durée | Difficulté | Statut     |
| ------ | ---------------------------------- | ------------------------ | ----- | ---------- | ---------- |
| **01** | Installation & Config Wazuh SIEM   | Wazuh, VM, Docker        | 3h    | ⭐⭐       | 📝 À venir |
| **02** | Règles de Détection Personnalisées | Wazuh Rules, Regex       | 4h    | ⭐⭐⭐     | 📝 À venir |
| **03** | Use Case : Compromission de Compte | Wazuh, Linux Logs        | 3h    | ⭐⭐⭐     | 📝 À venir |
| **04** | Use Case : Ransomware Detection    | File Monitoring, Alerts  | 5h    | ⭐⭐⭐⭐   | 📝 À venir |
| **05** | Use Case : Exfiltration de Données | Network Traffic Analysis | 4h    | ⭐⭐⭐⭐   | 📝 À venir |
| **06** | Analyse Logs Windows Event Viewer  | Windows Event IDs        | 3h    | ⭐⭐       | 📝 À venir |
| **07** | Playbook Phishing Complet          | Email Analysis, IOCs     | 3h    | ⭐⭐⭐     | 📝 À venir |
| **08** | Threat Hunting Proactif            | MITRE ATT&CK, KQL        | 5h    | ⭐⭐⭐⭐   | 📝 À venir |
| **09** | Dashboard SOC Opérationnel         | Kibana, Grafana          | 3h    | ⭐⭐⭐     | 📝 À venir |
| **10** | Reporting SOC Mensuel Automatisé   | Python, Jinja2           | 4h    | ⭐⭐⭐     | 📝 À venir |

---

## 🛠️ Outils utilisés

### SIEM

- **Wazuh** : SIEM open-source, détection d'intrusion, compliance
- **ELK Stack** : Elasticsearch + Logstash + Kibana pour analyse de logs
- **Splunk** : Plateforme d'analyse de données de sécurité

### IDS/IPS

- **Suricata** : Network IDS/IPS open-source
- **Snort** : Système de détection d'intrusion réseau

### Forensics & Analysis

- **Volatility** : Analyse de mémoire
- **Autopsy** : Digital forensics
- **Wireshark** : Analyse de trafic réseau

### Scripting

- **Python** : Automation et parsing de logs
- **Bash** : Scripts système pour correlation

---

## 📖 Ressources complémentaires

- [Wazuh Documentation](https://documentation.wazuh.com/)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [SANS SOC Survey](https://www.sans.org/white-papers/)
- [Splunk Security Essentials](https://splunkbase.splunk.com/app/3435/)

---

## 🎓 Certifications liées

- ✅ **CEH Fundamentals** (EC-Council)
- ✅ **IBM Cybersecurity Analyst**
- 🚧 **CompTIA Security+** (en cours)

---

## 💼 Cas d'usage professionnels

Ces labs préparent directement aux missions d'un **SOC Analyst Tier 1/2** :

- ✅ Monitoring temps réel des événements de sécurité
- ✅ Triage et qualification des alertes
- ✅ Investigation d'incidents
- ✅ Rédaction de rapports d'incident
- ✅ Amélioration continue des règles de détection

---

[← Retour à l'accueil]({{ site.baseurl }}/){: .btn .btn-outline }
