# Contributing to OSCI Community Referentiels

Merci de contribuer au projet OSCI ! Ce guide explique comment ajouter un nouveau referentiel de securite ou ameliorer un referentiel existant.

## Principes

- Chaque dossier = un referentiel (framework de securite)
- Chaque dossier contient un seul fichier `referentiel.json`
- Les referentiels sont importes par la plateforme OSCI via l'API GitHub
- Les contributions sont ouvertes a tous via Pull Request

## Ajouter un nouveau referentiel

### 1. Fork et clone

```bash
git clone https://github.com/<your-fork>/osci-referentiel.git
cd osci-referentiel
git checkout -b feat/mon-nouveau-referentiel
```

### 2. Creer le dossier

Le nom du dossier doit etre en kebab-case et refleter le framework :

```bash
mkdir mon-framework-v2
```

Exemples de noms : `pci-dss-v4`, `soc2-type2`, `rgpd-2016`, `anssi-hygiene`

### 3. Creer le fichier `referentiel.json`

Structure minimale :

```json
{
  "code": "MON_FRAMEWORK",
  "name": "Mon Framework v2",
  "description": "Description courte du framework",
  "version": "2.0",
  "type": "Internal",
  "domain": "Governance",
  "metadata": {},
  "controls": [
    {
      "code": "MF-01",
      "title": "Titre du controle",
      "description": null,
      "orderIndex": 0
    }
  ],
  "checklists": [
    {
      "title": "Ma Checklist",
      "version": "1.0",
      "domain": "Governance",
      "checklistType": "Compliance",
      "criticality": "High",
      "applicability": ["Project"],
      "description": "Ce que cette checklist evalue",
      "items": [
        {
          "question": "Est-ce que X est en place ?",
          "itemType": "YesNo",
          "weight": 1.0,
          "referenceType": "Internal",
          "reference": "Mon Framework v2 - Section 1.1",
          "expectedEvidence": null,
          "autoTaskTitle": null,
          "controlCode": "MF-01"
        }
      ]
    }
  ]
}
```

### 4. Valider le JSON

```bash
# Verifier que le JSON est valide
python -m json.tool mon-framework-v2/referentiel.json > /dev/null

# Ou avec Node.js
node -e "JSON.parse(require('fs').readFileSync('mon-framework-v2/referentiel.json'))"
```

### 5. Soumettre la PR

```bash
git add mon-framework-v2/
git commit -m "feat: add Mon Framework v2 referentiel"
git push origin feat/mon-nouveau-referentiel
```

Ouvrir une Pull Request sur le repository principal.

## Ameliorer un referentiel existant

### Ajouter des checklists

Vous pouvez ajouter de nouvelles checklists a un referentiel existant en ajoutant des entrees dans le tableau `checklists` du `referentiel.json`.

### Ajouter des controles

Ajoutez de nouveaux controles dans le tableau `controls`. Assurez-vous que l'`orderIndex` suit la sequence existante.

### Corriger des questions

Corrigez les fautes, ameliorez la clarte des questions, ajoutez des `expectedEvidence` manquants.

### Lier les items aux controles

Si des items ont `"controlCode": null`, vous pouvez les lier a un controle existant en renseignant le bon `controlCode`.

## Regles de contribution

### Code du referentiel

| Champ | Regles |
|-------|--------|
| `code` | Unique, MAJUSCULES_UNDERSCORE (ex: `ISO27001`, `PCI_DSS_V4`) |
| `name` | Nom officiel du framework avec version |
| `type` | `ISO`, `NIST`, `OWASP`, `CIS`, ou `Internal` pour les frameworks internes |
| `domain` | `Governance`, `SecurityInfra`, `SecurityCode`, `SecurityDevOps` |

### Controls

| Champ | Regles |
|-------|--------|
| `code` | Unique dans le referentiel, format libre (ex: `A.5.12`, `PR.AC-01`, `CTRL-01`) |
| `orderIndex` | Sequentiel a partir de 0 |

### Checklists

| Champ | Regles |
|-------|--------|
| `title` | Unique dans le referentiel |
| `checklistType` | `Compliance` (conformite) ou `Actionable` (actions concretes) |
| `criticality` | `Critical`, `High`, `Medium`, `Low` |
| `applicability` | Au moins un type d'objet cible |

### Items

| Champ | Regles |
|-------|--------|
| `question` | Claire, sans ambiguite, formulee comme une question fermee |
| `itemType` | `YesNo`, `Score`, `Evidence`, `AutoCheck` |
| `weight` | `1.0` par defaut, jusqu'a `3.0` pour les items critiques |
| `controlCode` | Doit correspondre a un `code` existant dans `controls`, ou `null` |

### Types d'applicabilite

Les checklists peuvent cibler ces types d'objets :

| Type | Description |
|------|-------------|
| `Infrastructure` | Serveurs, machines virtuelles, cloud |
| `Network` | Reseaux, firewalls, VPN |
| `Cluster` | Kubernetes, Docker Swarm |
| `Codebase` | Code source, repositories |
| `Pipeline` | CI/CD, build, deploy |
| `Project` | Projets, programmes, gouvernance |
| `DataAsset` | Donnees, bases de donnees, stockage |
| `AISystem` | Systemes IA, modeles ML |
| `Tool` | Outils de securite |
| `SystemTool` | API Gateway, Reverse Proxy, IdP, Secret Manager |
| `AgentTool` | Agents IA (Cursor, Copilot, ChatGPT, etc.) |
| `Server` | Serveurs specifiques |
| `Database` | Bases de donnees specifiques |

## Checklist avant soumission

- [ ] Le `code` du referentiel est unique
- [ ] Le JSON est valide (pas d'erreurs de syntaxe)
- [ ] Tous les `controlCode` dans les items referencent des codes existants dans `controls`
- [ ] Les `orderIndex` sont sequentiels (0, 1, 2, ...)
- [ ] Les questions sont claires et evaluables
- [ ] Le dossier est en kebab-case
- [ ] Pas d'informations personnelles (emails, noms, etc.) dans le JSON

## Besoin d'aide ?

Ouvrez une **Issue** sur ce repository pour poser vos questions ou proposer un nouveau framework avant de commencer le travail.
