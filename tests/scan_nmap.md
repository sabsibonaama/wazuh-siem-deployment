# Test — Détection d'un scan réseau (Nmap SYN Scan)

## Objectif

Valider le déclenchement de la règle personnalisée de détection de scan réseau via Suricata, en simulant une reconnaissance active de type SYN Scan (MITRE **T1595 — Active Scanning**).

## Environnement

- **Machine attaquante** : Kali Linux
- **Cible** : serveur Ubuntu supervisé (agent `ubuntu-agent2`)
- **Outil** : Nmap

## Commande utilisée

```bash
nmap -sS <IP_cible>
```

## Règle Wazuh déclenchée

| ID règle | Niveau | Technique MITRE | Description |
|----------|--------|------------------|--------------|
| 100601 | 6 | T1595 | Custom rule: MITRE T1595 - Nmap SYN Scan detected |

## Résultat

L'alerte Suricata associée à la signature personnalisée **« CUSTOM NMAP SYN Scan Detected »** a été correctement générée et remontée dans le Wazuh Dashboard, associée à l'agent `ubuntu-agent2`.

## Conclusion

Le test confirme que la chaîne de détection réseau (Suricata → décodeur Wazuh → règle de corrélation) fonctionne correctement pour identifier une activité de reconnaissance active.

---
📎 Référence : Section 3.7.1 du rapport de stage
