# Test — Détection d'attaques par force brute et corrélation avec l'escalade de privilèges (Linux)

## Objectif

Valider l'ensemble de la chaîne de détection Linux, depuis la simple tentative de connexion échouée jusqu'à la réponse automatisée (Active Response), en couvrant deux scénarios complémentaires :
- une simulation locale (commande `su`)
- une simulation distante (brute-force SSH via Hydra)

MITRE ATT&CK : **T1110 — Brute Force**, **T1548.003 — Abuse Elevation Control Mechanism: Sudo**, **T1078 — Valid Accounts**.

## Environnement

- **Machine attaquante** : Kali Linux
- **Cible** : machine Ubuntu Desktop supervisée (agent `ubuntu-agent2`)

---

## Scénario 1 — Simulation locale (`su`)

### Commandes utilisées

```bash
su user2
su root
```

Plusieurs tentatives ont été effectuées avec un mot de passe erroné, générant des échecs d'authentification répétés (`Authentication failure`).

### Règles déclenchées en cascade

| ID règle | Type | Niveau | Description |
|----------|------|--------|--------------|
| 5301 | Native | 5 | User missed the password to change UID |
| 5503 | Native | 5 | PAM: User login failed |
| 100115 | Custom | 10 | Possible password guessing attack |

### Réponse automatisée

Une **réponse active** (`active-response/bin/disable-account - add`) a automatiquement désactivé le compte concerné après dépassement du seuil défini. Vérification effectuée via :

```bash
sudo passwd --status user2
```

→ Résultat : statut `L` (Locked) confirmé. Le compte a ensuite été réactivé manuellement après investigation (`disable-account - delete`).

---

## Scénario 2 — Simulation distante (SSH + Hydra)

### Commande utilisée

```bash
hydra -l user1 -P /usr/share/wordlists/rockyou.txt ssh://<IP_cible>
```

### Règle déclenchée

| ID règle | Niveau | Technique MITRE | Description |
|----------|--------|------------------|--------------|
| 100100 | 10 | T1110 | Multiple failed login attempts |

---

## Corrélation et réponse automatisée

Les tentatives de force brute suivies d'un usage de `sudo` ont déclenché la règle de corrélation :

| ID règle | Niveau | Technique MITRE | Description |
|----------|--------|------------------|--------------|
| 100120 | 12 | T1078 | Correlation: Privilege escalation following brute-force login attempts |

Ce déclenchement a de nouveau entraîné la désactivation automatique du compte concerné.

## Notification email

Configuration `ossec.conf` : `email_notification = yes`, `email_alert_level = 9`, relais `postfix-relay`.

Email reçu confirmant le déclenchement de la règle **100100** (niveau 10) suite à la détection de tentatives de connexion répétées depuis une même adresse IP source.

## Conclusion

Le test valide le fonctionnement de bout en bout de la chaîne : détection → corrélation multi-étapes → réponse active automatisée → notification email.

---
📎 Référence : Section 3.7.2 du rapport de stage
