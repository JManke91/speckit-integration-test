---
artifact: plan-context
project_name: "lisa_test"
feature: "fahrten-matching-buchung"
derived_from:
  - wiki/entities/ecoride.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.0 - Projekt-Setup/1.0.1 - Projekt aufsetzen & Unterlagen hochladen/EcoRide_ProjektSetupApp_20260709084817479.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260702072544507.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260709085717997.md
compiled: 2026-07-13
---

# SpecKit Plan Context — lisa_test (EcoRide)

> Kompiliert aus dem Wiki-SSOT (COMPILE-SPECKIT).
> Verbindliche Technologieentscheidungen für die Verwendung mit /speckit.plan.

## Verwendung

Diese Datei enthält alle verbindlichen Technologieentscheidungen für das Projekt.
Kopiere den /speckit.plan Prompt am Ende dieser Datei und füge ihn zusammen mit der
spec.md in den /speckit.plan Befehl ein. Der Kontext stellt sicher, dass der KI-Agent
den korrekten Tech Stack verwendet.

## Extrahierte Technologieentscheidungen

### Frontend

| Entscheidung | Wert |
|---|---|
| Framework | React Native (iOS & Android) |
| Sprache | Nicht festgelegt |
| Drag-and-Drop | Nicht festgelegt |
| UI-Komponentenbibliothek | Nicht festgelegt |
| Weitere | Azure Maps (Kartendienst) |

### Backend

| Entscheidung | Wert |
|---|---|
| Framework | Nicht festgelegt |
| Laufzeitumgebung | Nicht festgelegt |
| Sprache | Nicht festgelegt |

### Datenbank

| Entscheidung | Wert |
|---|---|
| Technologie | PostgreSQL |
| ORM / Query-Builder | Nicht festgelegt |
| Migrations-Tool | Nicht festgelegt |
| Schema-Regeln | Nicht festgelegt |

### Authentifizierung

| Entscheidung | Wert |
|---|---|
| Mechanismus | Microsoft Entra ID (OAuth2/OIDC) |
| Token-Speicherung | Nicht festgelegt |
| Access Token Lifetime | Nicht festgelegt |
| Refresh Token Lifetime | Nicht festgelegt |
| Token Rotation | Nicht festgelegt |
| CSRF-Schutz | Nicht festgelegt |

### Echtzeit

| Entscheidung | Wert |
|---|---|
| Technologie | Nicht festgelegt |
| Muster | Nicht festgelegt |

### DevOps

| Entscheidung | Wert |
|---|---|
| CI/CD-System | Nicht festgelegt |
| Container | Nicht festgelegt |
| Image-Größe | Nicht festgelegt |
| Deployment-Ziel | Azure App Service |
| Healthcheck | Nicht festgelegt |

> Hinweise zur Extraktion („entschieden" vs. „erwähnt"): Die Session-Ablauffrist
> („z. B. nach 30 Tagen Inaktivität", SRS 2026-07-02 §3.1) ist als Beispiel formuliert
> und gilt daher nicht als festgelegte Token-Laufzeit. Echtzeit-Aktualisierung des
> Sitzplatzkontingents ist eine funktionale Anforderung (spec.md FR-005), ohne
> Technologie-Festlegung.

## /speckit.plan Prompt

```
Tech stack (verbindliche Entscheidungen aus dem Wiki-SSOT):
Frontend: React Native (iOS & Android)
Frontend (weitere): Azure Maps (Kartendienst)
Datenbank: PostgreSQL
Auth: Microsoft Entra ID (OAuth2/OIDC)
Deployment: Azure App Service

Step: Validate the plan

Audit the implementation plan. Cross-check that:
1. All FR-* requirements from spec.md are mapped to implementation tasks
2. All EC-* edge cases have corresponding error handling in the plan
3. All UI components and visual states are clearly specified in the plan
4. The constitution principles (especially performance budgets and security controls) are enforced via the plan's phase gates
```

*Quellen: EcoRide-Projektsetup (Tech-Stack), SRS 2026-07-02/2026-07-09, Wiki-Seite EcoRide*

<!-- extractedTech: {"frontend":{"framework":"React Native (iOS & Android)","language":null,"dnd":null,"ui":null,"other":["Azure Maps (Kartendienst)"]},"backend":{"framework":null,"runtime":null,"language":null},"database":{"technology":"PostgreSQL","orm":null,"migrations":null,"rules":[]},"auth":{"mechanism":"Microsoft Entra ID (OAuth2/OIDC)","storage":null,"accessTokenLifetime":null,"refreshTokenLifetime":null,"tokenRotation":false,"csrf":null},"realtime":{"technology":null,"pattern":null},"devops":{"cicd":null,"container":null,"imageSize":null,"deploymentTarget":"Azure App Service","healthcheck":null}} -->
