# Test — Détection d'une commande PowerShell encodée (Windows)

## Objectif

Valider la détection d'une technique couramment utilisée pour dissimuler une charge malveillante : l'exécution d'une commande PowerShell encodée en Base64.

MITRE ATT&CK : **T1059.001 — Command and Scripting Interpreter: PowerShell**.

## Environnement

- **Cible** : machine Windows 10 Pro supervisée (agent `win10pro-agent`)

## Commande utilisée

```powershell
powershell.exe -enc <chaîne_encodée_Base64>
```

## Règles déclenchées

| ID règle | Niveau | Technique MITRE | Description |
|----------|--------|------------------|--------------|
| 100130 | 12 | T1059.001 | Windows: Suspicious encoded PowerShell command |
| 92213 | 15 (native) | — | Executable file dropped in folder commonly used by malware |

## Corrélation

La règle personnalisée **100130** s'est déclenchée en corrélation avec la règle native **92213**, plus critique (niveau 15). L'objet de la notification email envoyée reflète le niveau d'alerte le plus élevé du lot, confirmant la capacité du système à prioriser les événements les plus critiques.

## Résultat

Alerte remontée dans le Wazuh Dashboard avec les événements Sysmon associés (création de processus, exécution suspecte). Notification email envoyée automatiquement.

## Conclusion

Le test confirme la capacité de la plateforme à détecter des techniques d'évasion courantes (encodage Base64) et à en évaluer correctement la criticité via corrélation de règles.

---
📎 Référence : Section 3.7.3 du rapport de stage
