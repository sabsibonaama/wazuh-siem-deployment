# Test — Détection d'une reconnaissance réseau (MITRE T1016 — Atomic Red Team)

## Objectif

Valider la détection de commandes de reconnaissance réseau via le framework **Atomic Red Team**, qui permet d'émuler des techniques d'attaque documentées dans le référentiel MITRE ATT&CK.

MITRE ATT&CK : **T1016 — System Network Configuration Discovery**.

## Environnement

- **Cible** : machine Windows 10 Pro supervisée (agent `win10pro-agent`)
- **Outil** : Atomic Red Team

## Commande utilisée

```powershell
Invoke-AtomicTest T1016 -TestNumber 1
```

Cette commande déclenche automatiquement une série de commandes de reconnaissance réseau (`arp -a`, `ipconfig`, etc.).

## Règle déclenchée

| ID règle | Niveau | Technique MITRE | Description |
|----------|--------|------------------|--------------|
| 100121 | 10 | T1016 | MITRE T1016 - Network Configuration Discovery detected |

## Détails de détection

L'analyse des journaux **Sysmon** confirme l'exécution de la commande `arp -a` par le processus `ARP.EXE`, intégrée au sein d'une séquence d'événements Sysmon (création de processus, exécution suspecte via l'invite de commandes).

## Notification email

Une notification par courrier électronique a confirmé le déclenchement de la règle 100121.

## Conclusion

Le test valide la capacité de la plateforme à détecter des comportements de reconnaissance interne, souvent utilisés par un attaquant après un accès initial pour cartographier l'environnement réseau.

---
📎 Référence : Section 3.7.4 du rapport de stage
