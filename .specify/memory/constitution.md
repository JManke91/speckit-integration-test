---
artifact: constitution
project_name: "lisa_test"
feature: "fahrten-matching-buchung"
version: "1.0.0"
derived_from:
  - wiki/entities/ecoride.md
  - wiki/concepts/fahrten-matching.md
  - wiki/concepts/dsgvo-compliance.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.0 - Projekt-Setup/1.0.1 - Projekt aufsetzen & Unterlagen hochladen/EcoRide_ProjektSetupApp_20260709084817479.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260702072544507.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260709085717997.md
compiled: 2026-07-13
---

# EcoRide (lisa_test) Constitution

## Core Principles

### I. Ausschließlich unternehmensinternes Single Sign-On (NON-NEGOTIABLE)

Die Anmeldung MUST ausschließlich über den unternehmensinternen Identitätsprovider per
OAuth2/OIDC erfolgen. Eine manuelle Anlage von Nutzern samt Passwort in der
Anwendungsdatenbank MUST NOT möglich sein (reines SSO, technisch ausgeschlossen). Bei der
Erstanmeldung MUST automatisch ein lokales Benutzerprofil aus den Basis-Claims (Name,
E-Mail) angelegt werden — ohne separates Registrierungsformular; bei jeder weiteren
Anmeldung MUST das Profil mit den aktuellen Claims synchronisiert werden. Nach Ablauf der
Token-Gültigkeit oder manuellem Logout MUST der Nutzer vollständig abgemeldet werden
(Single Sign-Out).

**Rationale**: Die Plattform ist ein rein internes Werkzeug; ein eigener Passwort-Store
wäre eine unnötige Angriffsfläche und verletzte das Prinzip der zentralen
Identitätsverwaltung der MusterFirma.

### II. DSGVO-Konformität und Datenminimierung (NON-NEGOTIABLE)

Die Plattform MUST der Datenschutz-Grundverordnung in vollem Umfang entsprechen,
insbesondere bei Bewegungsdaten und Profilinformationen. Aus dem Identitätsprovider
MUST NOT mehr bezogen werden als die zwingend für den Betrieb erforderlichen Daten
(Datenminimierung). Standortdaten MUST NOT ohne erteilte Datenschutzzustimmung des
Nutzers verarbeitet werden. Management-Berichte und Datenexporte MUST vollständig
aggregiert sein und MUST NOT Rückschlüsse auf das Fahrverhalten einzelner Mitarbeiter
erlauben.

**Rationale**: Bewegungs- und Pendeldaten sind hochsensibel; Nutzerakzeptanz und
Rechtssicherheit stehen und fallen mit hartem Datenschutz (Interview-Leitfaden Q10/Q14
zeigt Tracking-Bedenken als Adoptionsrisiko).

### III. Unveränderliche Protokollierung buchungsrelevanter Ereignisse

Alle Buchungs- und Storno-Statusänderungen MUST unveränderlich in der Datenbank
protokolliert werden. Bei jeder Stornierung MUST der Stornierungszeitpunkt dokumentiert
und alle betroffenen Parteien automatisch benachrichtigt werden.

**Rationale**: Die Protokolldaten sind die einzige Berechnungsgrundlage für CO2-Einspar-
und Parkplatz-Metriken; nachträgliche Veränderbarkeit würde die ESG-Berichte entwerten.

### IV. Messbare Nachhaltigkeitswirkung

Bei jeder abgeschlossenen Fahrt MUST das System die eingesparten CO2-Emissionen
berechnen und für das ESG-Reporting speichern. Die Berechnungen MUST auf konfigurierten
Referenzwerten (Emissionsfaktor, Parkplatz-Baseline) beruhen; fehlt ein Referenzwert,
MUST die Berechnung pausieren und als nicht verfügbar ausgewiesen werden — niemals mit
erfundenen Werten rechnen.

**Rationale**: Projektziel ist die messbare Senkung des CO₂-Fußabdrucks und die
Reduktion der Parkplatznot am Hauptstandort um 15 % — ohne belastbare Messung ist der
Projekterfolg nicht nachweisbar.

### V. Mobile-First-Portabilität

Die Lösung MUST für Endanwender nativ oder als Cross-Plattform-Anwendung auf
Mobilgeräten zur Verfügung stehen und MUST auf aktuellen iOS- (Version 15+) und
Android-Geräten (Version 11+) fehlerfrei installierbar und startbar sein.

**Rationale**: Die Zielgruppe (2.500 Mitarbeiter, 3 Standorte) nutzt die Plattform
unterwegs auf dem Arbeitsweg; der mobile Kanal ist der einzige spezifizierte Kanal für
Endanwender.

## Quality Gates

| Gate | Anforderung | Durchsetzung |
|------|-------------|-------------|
| DSGVO-Audit | Audit durch den internen Datenschutzbeauftragten vor MVP-Go-Live ohne kritische Befunde (Kategorie 'Blocker' oder 'High') | Interner Datenschutzbeauftragter, vor Go-Live |
| Plattform-Kompatibilität | Fehlerfreie Installation und Start auf iOS 15+ und Android 11+ | Release-Test je Zielplattform |
| Datenaggregation | Dashboards/Exporte ausschließlich aggregiert (z. B. Monats-/Standortebene), keine Personenrückschlüsse | Review vor Freigabe von Reporting-Funktionen |
| Audit-Protokoll | Buchungs-/Storno-Statusänderungen unveränderlich gespeichert | Code Review + Datenmodell-Review |

## Governance

- **Autorität**: Diese Constitution hat für alle EcoRide-Features Vorrang vor
  Team-Konventionen und informellen Vereinbarungen.
- **Änderungsverfahren**: Schriftlicher Vorschlag → Review/Freigabe per Pull Request →
  Versionsbump → Re-Compile abhängiger Artefakte.
- **Versionierung**: Semantische Versionierung — MAJOR bei Entfernung/Neudefinition von
  Prinzipien, MINOR bei neuen Prinzipien oder Gates, PATCH bei Klarstellungen.
- **Compliance**: Feature-Specs referenzieren die relevanten Prinzipien im
  Dependencies-Abschnitt; Abweichungen MUST formell begründet werden.

**Version**: 1.0.0 | **Ratified**: 2026-07-13 | **Last Amended**: 2026-07-13
