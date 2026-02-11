# OSCI Community Referentiels

Community-maintained security frameworks, controls, and checklists for the [OSCI Platform](https://github.com/osci-platform/security-checklist-idp).

## Available Referentiels

| Folder | Framework | Controls | Checklists | Domain |
|--------|-----------|----------|------------|--------Dans|
| `iso-27001/` | ISO/IEC 27001:2022 | 11 | 25 | Governance |
| `nist-csf/` | NIST CSF 2.0 | 9 | 46 | Governance |
| `owasp-top10/` | OWASP Top 10 (2021) | 10 | 14 | Security Code |
| `cis-controls-v8/` | CIS Controls v8 | 7 | 7 | Security Infra |

## How to Use

From the OSCI web application:

1. Go to **Referentiels** > click **Community**
2. Browse available frameworks and their checklists
3. Click **Import** on the referentiel or individual checklists you need
4. Imported items appear in your Referentiels list with linked controls

## Contributing a New Referentiel

1. Fork this repository
2. Create a folder named after the framework (e.g., `pci-dss-v4/`)
3. Add a `referentiel.json` file following the format below
4. Open a Pull Request

### JSON Format

Each folder contains a single `referentiel.json`:

```json
{
  "code": "UNIQUE_CODE",
  "name": "Framework Display Name",
  "description": "Brief description of the framework",
  "version": "1.0",
  "type": "ISO | NIST | OWASP | CIS | Internal",
  "domain": "Governance | SecurityInfra | SecurityCode | SecurityDevOps",
  "metadata": {},
  "controls": [
    {
      "code": "CTRL-01",
      "title": "Control title",
      "description": "Optional longer description",
      "orderIndex": 0
    }
  ],
  "checklists": [
    {
      "title": "Checklist Title",
      "version": "1.0",
      "domain": "SecurityInfra",
      "checklistType": "Compliance | Actionable",
      "criticality": "Critical | High | Medium | Low",
      "applicability": ["Infrastructure", "Codebase", "Network", "Cluster"],
      "description": "What this checklist assesses",
      "items": [
        {
          "question": "Is X configured correctly?",
          "itemType": "YesNo | Score | Evidence | AutoCheck",
          "weight": 1.0,
          "referenceType": "ISO | NIST | OWASP | CIS | Internal | null",
          "reference": "Reference standard identifier or null",
          "expectedEvidence": "Description of expected evidence or null",
          "autoTaskTitle": "Task title if non-compliant or null",
          "controlCode": "CTRL-01 or null"
        }
      ]
    }
  ]
}
```

### Field Reference

#### Referentiel

| Field | Required | Description |
|-------|----------|-------------|
| `code` | Yes | Unique identifier (e.g., `ISO27001`, `NIST_CSF`). Must not conflict with existing referentiels. |
| `name` | Yes | Display name |
| `description` | No | Brief description of the framework |
| `version` | Yes | Version string |
| `type` | Yes | Framework type: `ISO`, `NIST`, `OWASP`, `CIS`, `Internal` |
| `domain` | Yes | Primary domain: `Governance`, `SecurityInfra`, `SecurityCode`, `SecurityDevOps` |
| `metadata` | No | Arbitrary key-value metadata |

#### Controls

| Field | Required | Description |
|-------|----------|-------------|
| `code` | Yes | Unique control code within this referentiel (e.g., `A.5.12`, `PR.AC-01`) |
| `title` | Yes | Control title |
| `description` | No | Detailed description |
| `orderIndex` | Yes | Display order (0-based) |

#### Checklist Items

| Field | Required | Description |
|-------|----------|-------------|
| `question` | Yes | The assessment question |
| `itemType` | Yes | `YesNo` (pass/fail), `Score` (0-100%), `Evidence` (file upload), `AutoCheck` (automated) |
| `weight` | Yes | Scoring weight (default `1.0`, higher = more impact) |
| `referenceType` | No | Source standard type |
| `reference` | No | Reference identifier (e.g., `ISO 27001:2022 A.8.22`) |
| `expectedEvidence` | No | Description of what evidence to provide |
| `autoTaskTitle` | No | If set, a task is created automatically when item is non-compliant |
| `controlCode` | No | Links this item to a framework control by its `code`. Set to `null` if no link. |

### Applicability Types

Items in a checklist can target different object types:

`Infrastructure`, `Network`, `Cluster`, `Codebase`, `Pipeline`, `Project`, `DataAsset`, `AISystem`, `Tool`, `SystemTool`, `AgentTool`, `Server`, `Database`

### Validation Checklist

Before submitting a PR, verify:

- [ ] `code` is unique and not already used by another referentiel
- [ ] All `controlCode` values in items reference valid control codes defined in the `controls` array
- [ ] JSON is valid (run through a JSON linter)
- [ ] `orderIndex` values are sequential starting from 0
- [ ] Questions are clear and actionable

## Licence
Ce référentiel est distribué sous licence MIT.  
Voir [LICENSE](./LICENSE) pour plus d’informations.

