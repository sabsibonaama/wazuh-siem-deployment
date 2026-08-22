# 🛡️ Déploiement et personnalisation d'une plateforme SIEM basée sur Wazuh

Projet réalisé dans le cadre d'un stage de fin de 1ère année (Cycle Ingénieur) en **Cybersécurité et Confiance Numérique** (du 01 au 31 juillet 2026).

## 📋 Contexte

Ce projet vise à centraliser la collecte des journaux d'événements, automatiser leur analyse et améliorer la détection des incidents de sécurité au sein d'une infrastructure de test, à l'aide de la plateforme open source **Wazuh**.

## 🏗️ Architecture

- **Serveur Ubuntu** hébergeant les composants Wazuh conteneurisés (Docker)
- **Wazuh Manager** — analyse et corrélation des événements
- **Wazuh Indexer** (OpenSearch) — stockage et indexation des logs
- **Wazuh Dashboard** — interface de supervision
- **3 agents connectés** : Ubuntu Desktop, Windows 10 Pro, Kali Linux (poste attaquant)
- **Suricata** — détection d'intrusion réseau (IDS)
- **Postfix** — relais SMTP pour les notifications par email

## 🎯 Objectifs

- Déployer Wazuh via Docker sur un serveur Ubuntu
- Intégrer plusieurs agents multi-OS
- Développer des règles de détection personnalisées basées sur **MITRE ATT&CK**
- Valider les règles via des scénarios d'attaque simulés
- Automatiser les notifications d'alertes critiques

## 🔍 Règles de détection développées

| ID règle | Niveau | Technique MITRE | Objectif |
|----------|--------|------------------|----------|
| 100100 | 10 | T1110 | Détection de force brute (échecs de connexion) |
| 100102 | 8 | T1548.003 | Traçabilité des usages de `sudo` |
| 100120 | 12 | T1078 | Corrélation brute-force → escalade de privilèges |
| 100115 | 6 | T1110 | Détection de password guessing (PAM) |
| 100130 | 12 | T1059.001 | Commande PowerShell encodée suspecte |
| 100121 | 10 | T1016 | Reconnaissance de configuration réseau |
| 100601 | 6 | T1595 | Scan Nmap SYN |
| 100602 | 6 | T1595 | Scan Nmap NULL |
| 100603 | 6 | T1071 | Connexion Netcat détectée |

## 🧪 Scénarios de test validés

- Scan réseau SYN (Nmap)
- Password guessing local (`su`)
- Escalade de privilèges (`su` + `sudo`)
- Brute-force SSH (connexions manuelles + Hydra)
- Commande PowerShell encodée en Base64
- Reconnaissance réseau (Atomic Red Team — T1016)

Tous les scénarios ont déclenché les règles attendues, avec envoi automatique de notifications email pour les alertes critiques (niveau ≥ 9).

## 🛠️ Stack technique

| Composant | Version |
|-----------|---------|
| Ubuntu Server | 24.04 LTS |
| Wazuh (Manager/Indexer/Dashboard) | 4.14.5 |
| Docker  | 29.1.3|
| docker-compose | 1.29.2 |
| Suricata | 7.0.3 |
| Postfix | 3.7.2 |
| Kali Linux | 2026.1 |
| Windows 10 Pro | 22H2 |

## 📁 Structure du dépôt
