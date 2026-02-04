---
layout: default
title: "Lab 41 - Gestion Utilisateurs & Permissions Linux"
parent: Réseaux & Systèmes
nav_order: 1
---

# Lab 41 - Gestion des Utilisateurs et Permissions sous Linux

{: .no_toc }

## Table des matières

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## 🎯 Objectifs du Lab

Ce lab pratique permet de maîtriser la **gestion complète des utilisateurs** et des **permissions** sous Linux, compétences essentielles pour tout administrateur système.

**Compétences développées :**

- ✅ Création et gestion d'utilisateurs et de groupes
- ✅ Configuration des permissions (chmod, chown)
- ✅ Gestion des répertoires partagés
- ✅ Utilisation du skeleton (`/etc/skel`)
- ✅ Architecture multi-services avec droits différenciés

---

## 🏗️ Architecture du Lab

```
Entreprise (3 services)
│
├── Service Marketing
│   ├── jdupond (Chef) ────────┐
│   ├── jmartin                │
│   └── fmalko                 │
│                               │
├── Service Production          ├──→ Groupe "chefs"
│   ├── mmartinoli (Chef) ─────┤     (Accès partagechefs/)
│   └── dkeita                 │
│                               │
└── Service Secrétariat         │
    ├── edaniel (Chef) ────────┘
    └── hpagnac

Dossiers Partagés :
- /home/partagetous      → Lecture pour tous, écriture pour jdupond
- /home/partagechefs     → Accès réservé aux chefs
- /home/marketing/partagemark  → Accès groupe marketing
- /home/prod/partageprod       → Accès groupe production
- /home/secretaires/partagesec → Accès groupe secretariat
```

---

## 📋 Prérequis

- Machine Linux (Ubuntu 20.04+ ou CentOS 7+)
- Accès root
- Éditeur de texte (nano, vim)

---

## 🔧 Partie 1 : Concepts Fondamentaux

### 1.1. Les Fichiers de Base de Données

#### `/etc/passwd` - Annuaire des utilisateurs

Chaque ligne représente un utilisateur :

```bash
pmayet:x:508:511:Pierre Mayet:/home/etudiant/ts2igb/pmayet:/bin/bash
```

**Décomposition des 7 champs :**

| Champ | Valeur             | Signification                                |
| ----- | ------------------ | -------------------------------------------- |
| 1     | `pmayet`           | **Login** (nom d'utilisateur)                |
| 2     | `x`                | **Mot de passe** (stocké dans `/etc/shadow`) |
| 3     | `508`              | **UID** (User ID - identifiant numérique)    |
| 4     | `511`              | **GID** (Group ID - groupe principal)        |
| 5     | `Pierre Mayet`     | **Commentaire** (nom complet)                |
| 6     | `/home/.../pmayet` | **Home Directory** (répertoire personnel)    |
| 7     | `/bin/bash`        | **Shell** (interpréteur de commandes)        |

{: .important }

> **Root est toujours UID 0** - C'est le compte administrateur tout-puissant.

#### `/etc/group` - Annuaire des groupes

```bash
ts2igb:x:511:membre1,membre2
```

| Champ | Valeur            | Signification                   |
| ----- | ----------------- | ------------------------------- |
| 1     | `ts2igb`          | Nom du groupe                   |
| 2     | `x`               | Mot de passe (rarement utilisé) |
| 3     | `511`             | GID (identifiant numérique)     |
| 4     | `membre1,membre2` | Membres secondaires             |

---

### 1.2. Les Permissions (rwx)

Chaque fichier/dossier a **3 niveaux de permissions** pour **3 types d'utilisateurs** :

```bash
-rwxr-xr--
│││││││││└─→ Other (Autres) : r-- (Lecture seule)
││││││└└└──→ Group (Groupe) : r-x (Lecture + Exécution)
│└└└└└─────→ User (Proprio) : rwx (Tout)
└──────────→ Type (- = fichier, d = dossier)
```

**Tableau des valeurs octales :**

| Symbole | Valeur | Binaire | Signification       |
| ------- | ------ | ------- | ------------------- |
| `r--`   | 4      | 100     | Lecture seule       |
| `-w-`   | 2      | 010     | Écriture seule      |
| `--x`   | 1      | 001     | Exécution seule     |
| `rw-`   | 6      | 110     | Lecture + Écriture  |
| `r-x`   | 5      | 101     | Lecture + Exécution |
| `rwx`   | 7      | 111     | Tous les droits     |
| `---`   | 0      | 000     | Aucun droit         |

**Exemples courants :**

```bash
chmod 755 fichier  # rwxr-xr-x (Standard pour programmes)
chmod 644 fichier  # rw-r--r-- (Standard pour fichiers)
chmod 700 dossier  # rwx------ (Dossier privé)
chmod 770 dossier  # rwxrwx--- (Dossier partagé en groupe)
```

---

## 🛠️ Partie 2 : Application Pratique 1

### Configuration Initiale

**Se connecter en root :**

```bash
su -
# OU
sudo -i
```

---

### Exercice 1 : Création de Groupes

**Q1. Créer les groupes `sav` et `syndicat`**

```bash
groupadd sav
groupadd syndicat
```

**Q2. Vérifier la création**

```bash
tail /etc/group
```

**Résultat attendu :**

```
sav:x:1001:
syndicat:x:1002:
```

---

### Exercice 2 : Création d'Utilisateurs Basique

**Q3. Créer Pierre CAPLIEZ (pcapliez), groupe syndicat**

```bash
useradd -g syndicat -c "Pierre CAPLIEZ" -m pcapliez
```

**Explication des options :**

- `-g syndicat` : Groupe principal
- `-c "Pierre CAPLIEZ"` : Commentaire (nom complet)
- `-m` : Créer le répertoire personnel

**Q4. Vérifier la création**

```bash
grep pcapliez /etc/passwd
```

**Q5. Répertoire de base ?**

```bash
ls -ld /home/pcapliez
```

**Réponse :** `/home/pcapliez`

**Q6. Shell par défaut ?**

**Réponse :** `/bin/bash`

**Q8. Définir le mot de passe**

```bash
passwd pcapliez
# Entrer "pierro" deux fois
```

**Q9. Tester la connexion**

```bash
su - pcapliez
id
# uid=1001(pcapliez) gid=1002(syndicat) groups=1002(syndicat)
exit
```

---

### Exercice 3 : Utilisateur avec Répertoire Personnalisé

**Q10. Créer Lise SCULLY avec un emplacement spécifique**

```bash
# Créer le dossier parent
mkdir -p /home/secretaire

# Créer l'utilisateur
useradd -g sav -G syndicat -d /home/secretaire/lscully -m -c "Lise SCULLY" lscully
```

**Explication :**

- `-g sav` : Groupe principal
- `-G syndicat` : Groupe **secondaire** (supplémentaire)
- `-d /home/secretaire/lscully` : Répertoire personnalisé
- `-m` : Forcer la création du répertoire

**Q11. Mot de passe**

```bash
passwd lscully
# Entrer "trustnoone"
```

**Q13. Groupe principal de lscully ?**

```bash
grep lscully /etc/passwd
# Réponse : sav (c'est le 4ème champ)
```

**Q14. Voir TOUS ses groupes**

```bash
groups lscully
# OU
id lscully
```

**Résultat :** `sav syndicat`

---

### Exercice 4 : Modification d'Utilisateur

**Q15. Ajouter pcapliez au groupe sav (SANS perdre syndicat)**

```bash
usermod -aG sav pcapliez
```

{: .warning }

> **ATTENTION :** Le `-a` (append) est **crucial** ! Sans lui, `-G` **écrase** tous les groupes secondaires existants.

**Q16. Vérifier**

```bash
groups pcapliez
# Résultat : syndicat sav
```

---

### Exercice 5 : Le Skeleton (`/etc/skel`)

**Q17-18. Fichiers par défaut dans les home ?**

```bash
ls -la /home/pcapliez
```

**Réponse :** `.bash_logout`, `.bash_profile`, `.bashrc`

**Q19. Fichiers dans /etc/skel ?**

```bash
ls -la /etc/skel
```

**Résultat :** Les mêmes fichiers cachés

{: .note }

> **Mécanisme :** `/etc/skel` = "Skeleton" (Squelette)  
> À chaque `useradd -m`, Linux **copie** tout le contenu de `/etc/skel` vers le nouveau `/home/user`.

**Q20. Créer un fichier test dans skel**

```bash
touch /etc/skel/essais.txt
```

**Q21. Créer un nouvel utilisateur**

```bash
useradd -m blapointe
```

**Q22. Vérifier le contenu de son home**

```bash
ls -la /home/blapointe
```

**Résultat :** Le fichier `essais.txt` est présent !

**Q23. Intérêt pratique de /etc/skel ?**

**Réponse :**

- Distribuer automatiquement des fichiers à tous les nouveaux utilisateurs
- Exemple : règlement intérieur, fond d'écran par défaut, configurations
- Gain de temps énorme dans une entreprise

**Q24. Groupe de blapointe ?**

```bash
id blapointe
```

**Réponse :** Par défaut, Linux crée un groupe avec le même nom : `blapointe`

**Q25. Supprimer blapointe (et son répertoire)**

```bash
userdel -r blapointe
```

{: .important }

> **Toujours utiliser `-r`** pour éviter les dossiers fantômes qui occupent de l'espace.

---

## 🏢 Partie 3 : Application Pratique 2 (Entreprise)

### Scénario

Une entreprise a **3 services** (Marketing, Production, Secrétariat).  
Chaque service a un **chef** et des **employés**.

**Règles à implémenter :**

1. Chaque service a son dossier partagé (accessible qu'au service)
2. Les chefs ont un dossier commun (`partagechefs`)
3. Tout le monde peut lire `partagetous`, seul `jdupond` peut écrire
4. Les dossiers personnels sont **privés** (700)

---

### Étape 1 : Créer les Groupes

```bash
groupadd marketing
groupadd production
groupadd secretariat
groupadd chefs
groupadd tous
```

---

### Étape 2 : Créer les Dossiers Partagés

#### Dossiers parents

```bash
mkdir /home/marketing
mkdir /home/prod
mkdir /home/secretaires
```

#### Dossiers partagés par service

**Marketing :**

```bash
mkdir /home/marketing/partagemark
chgrp marketing /home/marketing/partagemark
chmod 770 /home/marketing/partagemark
```

**Production :**

```bash
mkdir /home/prod/partageprod
chgrp production /home/prod/partageprod
chmod 770 /home/prod/partageprod
```

**Secrétariat :**

```bash
mkdir /home/secretaires/partagesec
chgrp secretariat /home/secretaires/partagesec
chmod 770 /home/secretaires/partagesec
```

**Explication `chmod 770` :**

- `7` (Propriétaire) : rwx (tout)
- `7` (Groupe) : rwx (tout)
- `0` (Autres) : --- (rien)

→ Seul le service concerné peut accéder.

#### Dossier des chefs

```bash
mkdir /home/partagechefs
chgrp chefs /home/partagechefs
chmod 770 /home/partagechefs
```

#### Dossier commun (à créer après jdupond)

```bash
mkdir /home/partagetous
# On configurera après la création de jdupond
```

---

### Étape 3 : Créer les Utilisateurs

#### Service Marketing

**jdupond (Chef) :**

```bash
useradd -g marketing -G chefs,tous -d /home/marketing/jdupond -m jdupond
passwd jdupond

# Maintenant on peut configurer partagetous
chown jdupond:tous /home/partagetous
chmod 750 /home/partagetous
```

**Explication `chmod 750` :**

- `7` (jdupond) : Lecture, écriture, exécution
- `5` (groupe "tous") : Lecture et exécution **SEULEMENT** (pas d'écriture)
- `0` (Autres) : Rien

**jmartin et fmalko (Employés) :**

```bash
useradd -g marketing -G tous -d /home/marketing/jmartin -m jmartin
useradd -g marketing -G tous -d /home/marketing/fmalko -m fmalko

passwd jmartin
passwd fmalko
```

#### Service Production

**mmartinoli (Chef) :**

```bash
useradd -g production -G chefs,tous -d /home/prod/mmartinoli -m mmartinoli
passwd mmartinoli
```

**dkeita (Employé) :**

```bash
useradd -g production -G tous -d /home/prod/dkeita -m dkeita
passwd dkeita
```

#### Service Secrétariat

**edaniel (Chef) :**

```bash
useradd -g secretariat -G chefs,tous -d /home/secretaires/edaniel -m edaniel
passwd edaniel
```

**hpagnac (Employé) :**

```bash
useradd -g secretariat -G tous -d /home/secretaires/hpagnac -m hpagnac
passwd hpagnac
```

---

### Étape 4 : Sécuriser les Dossiers Personnels

**Règle :** Chaque utilisateur doit être le SEUL à accéder à son dossier.

```bash
# Marketing
chmod 700 /home/marketing/jdupond
chmod 700 /home/marketing/jmartin
chmod 700 /home/marketing/fmalko

# Production
chmod 700 /home/prod/mmartinoli
chmod 700 /home/prod/dkeita

# Secrétariat
chmod 700 /home/secretaires/edaniel
chmod 700 /home/secretaires/hpagnac
```

---

## 🧪 Tests et Vérifications

### Test 1 : Vérifier les groupes d'un utilisateur

```bash
id jdupond
# Attendu : uid=... gid=...(marketing) groups=...(marketing),...(chefs),...(tous)
```

### Test 2 : Tester les accès aux dossiers partagés

**En tant que jmartin (Marketing) :**

```bash
su - jmartin

# Peut-il accéder au dossier marketing ?
cd /home/marketing/partagemark  # ✅ OUI
touch test.txt                   # ✅ OUI (peut écrire)

# Peut-il accéder au dossier production ?
cd /home/prod/partageprod        # ❌ NON (Permission denied)

# Peut-il accéder au dossier chefs ?
cd /home/partagechefs            # ❌ NON (pas dans le groupe chefs)

exit
```

**En tant que jdupond (Chef Marketing) :**

```bash
su - jdupond

# Peut-il accéder au dossier chefs ?
cd /home/partagechefs            # ✅ OUI (il est chef)

# Peut-il écrire dans partagetous ?
cd /home/partagetous
touch fichier_chef.txt           # ✅ OUI (il est propriétaire)

exit
```

**En tant que hpagnac (Secrétariat, non chef) :**

```bash
su - hpagnac

# Peut-il LIRE dans partagetous ?
ls /home/partagetous             # ✅ OUI (groupe "tous" a r-x)

# Peut-il ÉCRIRE dans partagetous ?
touch /home/partagetous/test.txt # ❌ NON (Permission denied)

exit
```

---

## 📊 Tableau Récapitulatif des Droits

| Dossier                       | Propriétaire | Groupe    | Permissions | Qui peut accéder ?                       |
| ----------------------------- | ------------ | --------- | ----------- | ---------------------------------------- |
| `/home/marketing/jdupond`     | jdupond      | marketing | `700`       | jdupond uniquement                       |
| `/home/marketing/partagemark` | root         | marketing | `770`       | Tous les membres du groupe marketing     |
| `/home/partagechefs`          | root         | chefs     | `770`       | jdupond, mmartinoli, edaniel             |
| `/home/partagetous`           | jdupond      | tous      | `750`       | Lecture pour tous, écriture pour jdupond |

---

## 🎓 Compétences Démontrées

### Techniques

- ✅ Maîtrise des commandes `useradd`, `usermod`, `userdel`
- ✅ Gestion des groupes primaires et secondaires
- ✅ Calcul et application des permissions octales
- ✅ Utilisation de `chown` et `chgrp`
- ✅ Compréhension du mécanisme `/etc/skel`

### Professionnelles

- ✅ Modélisation d'une organisation multi-services
- ✅ Application du principe du moindre privilège
- ✅ Séparation des responsabilités (chefs vs employés)
- ✅ Documentation claire et reproductible

---

## 📚 Ressources Complémentaires

- [Linux User Management - Red Hat Docs](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups)
- [File Permissions Explained - DigitalOcean](https://www.digitalocean.com/community/tutorials/linux-permissions-basics-and-how-to-use-umask-on-a-vps)
- [Understanding /etc/skel](https://www.thegeekdiary.com/understanding-the-etc-skel-directory-in-linux/)

---

## 🔗 Liens

- [← Retour à Réseaux & Systèmes](/labs/reseaux-systemes/)
- [← Retour à l'accueil](/labs/)
- [📧 Poser une question](mailto:issamono62@gmail.com)

---

{: .fs-3 }
**Temps de réalisation :** 2-3 heures  
**Niveau :** Intermédiaire  
**Dernière mise à jour :** Février 2026
