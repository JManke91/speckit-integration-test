---
artifact: spec
project_name: "lisa_test"
feature: "fahrten-matching-buchung"
derived_from:
  - wiki/entities/ecoride.md
  - wiki/concepts/fahrten-matching.md
  - wiki/concepts/dsgvo-compliance.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.0 - Projekt-Setup/1.0.1 - Projekt aufsetzen & Unterlagen hochladen/EcoRide_ProjektSetupApp_20260709084817479.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.2 - Erfassung (Ingestion)/1.2.2 - Anforderungs-Fragebögen/lisa_test_LeitfadeninterviewsApp_20260709085323916.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260702072544507.md
  - raw/lisa_test/1 - Anforderungen und Planung/1.3 - Synthese/1.3.1 - Anforderungsdoku Erzeugen (SRS Draft)/Standard_AnforderungsdokumentationApp_20260709085717997.md
compiled: 2026-07-13
---

# Feature Specification: Fahrten-Matching und Buchung

**Feature Branch**: `fahrten-matching-buchung`
**Created**: 2026-07-13
**Status**: Draft
**Source Requirement Documents**: SRS 2026-07-09 (§3.2), SRS 2026-07-02 (§3.2, §3.1), Projekt-Setup 2026-07-09, Interview-Leitfaden 2026-07-09
**Specification Mode**: STANDARD

## Overview

Kern-Feature der EcoRide-Plattform: Mitarbeiter der MusterFirma finden über die Eingabe
von Startpunkt, Ziel und Abfahrtszeit passende Mitfahrgelegenheiten, buchen diese mit
wenigen Klicks verbindlich und stornieren bei Bedarf transparent. Das Feature erhöht die
PKW-Auslastung, reduziert die Parkplatznot am Hauptstandort (Ziel: −15 %) und liefert die
Datengrundlage für CO2-Einspar- und Parkplatz-Metriken.

## User Scenarios & Testing

### User Story 1 – Fahrt finden und buchen (Priority: P1)

Ein authentifizierter Mitarbeiter gibt Start, Ziel und gewünschte Abfahrtszeit ein (oder
nutzt GPS für den Startort), erhält eine Liste passender Fahrten mit Fahrer-Details und
Abweichungsangaben und bucht eine Fahrt direkt aus der Detailansicht.

**Acceptance Scenarios:**
1. **Given** ein authentifizierter Nutzer mit erteilter Standort-Zustimmung und mindestens
   einem passenden Fahrtenangebot, **When** er Start, Ziel und Abfahrtszeit eingibt,
   **Then** zeigt das System innerhalb von 3 Sekunden alle Fahrten an, die maximal 2 km
   von Start/Ziel und 30 Minuten von der Wunschzeit abweichen.
2. **Given** eine angezeigte Fahrt in der Suchergebnis-Detailansicht, **When** der Nutzer
   die Buchung durchführt, **Then** ist die Buchung mit maximal 3 Klicks verbindlich
   abgeschlossen und das Sitzplatzkontingent der Fahrt sinkt in Echtzeit.
3. **Given** ein Nutzer ohne GPS-Berechtigung, **When** er die Fahrtsuche öffnet,
   **Then** fordert das System zur manuellen Eingabe von Start- und Zieladresse auf.

### User Story 2 – Fahrt anbieten (Priority: P2)

Ein Mitarbeiter in der Fahrerrolle bietet eine Fahrt mit Startadresse, Zieladresse,
Datum, Uhrzeit und der Anzahl verfügbarer Plätze an, damit Mitfahrer sie im Matching
finden können.

**Acceptance Scenarios:**
1. **Given** ein authentifizierter Nutzer in der Fahrerrolle, **When** er eine Fahrt mit
   gültigen Angaben (Abfahrtszeit mindestens 15 Minuten in der Zukunft, 1–4 Plätze)
   anlegt, **Then** speichert das System das Fahrtenangebot und stellt es dem
   Matching-Algorithmus bereit.
2. **Given** eine Fahrtanlage mit Abfahrtszeit weniger als 15 Minuten in der Zukunft,
   **When** der Nutzer speichern will, **Then** weist das System die Eingabe mit einem
   Hinweis auf den gültigen Zeitbereich zurück.

### User Story 3 – Fahrt stornieren (Priority: P2)

Ein Nutzer storniert eine gebuchte Fahrt über einen eindeutig sichtbaren
Stornieren-Button in seiner Buchungsübersicht; alle betroffenen Parteien werden
automatisch benachrichtigt.

**Acceptance Scenarios:**
1. **Given** eine bestehende Buchung, **When** der Nutzer 'Stornieren' wählt, **Then**
   zeigt das System zuerst einen Bestätigungs-Dialog mit Warnung über mögliche
   Konsequenzen, und storniert erst nach Bestätigung.
2. **Given** ein Fahrer mit gebuchten Mitfahrern, **When** der Fahrer die Fahrt
   storniert, **Then** storniert das System automatisch die Buchungen der Mitfahrer und
   sendet diesen innerhalb von 1 Minute eine Push-Benachrichtigung mit
   Alternativvorschlägen.

## Requirements

### Functional Requirements

- **FR-001**: System MUST dem Endanwender die Fahrtsuche über Startpunkt, Zieladresse
  und Abfahrtszeit ermöglichen (manuelle Eingabe oder GPS-Auswahl für den Startort).
- **FR-002**: Der Matching-Algorithmus MUST ausschließlich Fahrten vorschlagen, die
  zeitlich und räumlich innerhalb der definierten Systemtoleranzen liegen (maximal 2 km
  Abweichung von Start und Ziel, maximal 30 Minuten von der Wunschzeit).
- **FR-003**: System MUST Fahrern das Anlegen von Fahrtenangeboten mit Rolle,
  Startadresse, Zieladresse, Datum, Uhrzeit (mindestens 15 Minuten in der Zukunft) und
  1–4 verfügbaren Sitzplätzen ermöglichen.
- **FR-004**: System MUST die verbindliche Buchung einer angezeigten Fahrt aus der
  Suchergebnis-Detailansicht mit maximal 3 Klicks ermöglichen (siehe CL-001).
- **FR-005**: Jede erfolgreiche Buchung MUST das Sitzplatzkontingent der Fahrt in
  Echtzeit aktualisieren; ein Overcommit MUST NOT möglich sein.
- **FR-006**: System MUST die Stornierung einer gebuchten Fahrt über einen eindeutig
  sichtbaren Button 'Stornieren' in der Buchungsübersicht ermöglichen.
- **FR-007**: Vor der endgültigen Stornierung MUST der Nutzer einen Bestätigungs-Dialog
  (Warnung über mögliche Konsequenzen/Regelungen) akzeptieren.
- **FR-008**: Bei jeder Stornierung MUST das System automatisch eine Push- oder
  E-Mail-Benachrichtigung an alle betroffenen Parteien versenden, die den
  Stornierungszeitpunkt dokumentiert.
- **FR-009**: Alle Buchungs- und Storno-Statusänderungen MUST unveränderlich in der
  Datenbank protokolliert werden (Berechnungsgrundlage für CO2- und Parkplatz-Metriken).
- **FR-010**: Bei jeder abgeschlossenen Fahrt MUST das System die eingesparten
  CO2-Emissionen berechnen und für die Management-ESG-Berichte speichern.
- **FR-011**: Findet der Algorithmus kein Match, SHOULD das System die Erstellung eines
  Such-Alarms für die Route anbieten.
- **FR-012**: System SHOULD bei zu knapper Abfahrtszeit einen Warnhinweis anzeigen.
- **FR-013**: System MAY haptisches Feedback (Vibration) bei erfolgreicher Buchung oder
  Storno-Bestätigung geben.
- **FR-014**: System MUST Standortdaten nur verarbeiten, wenn der Nutzer die
  erforderliche DSGVO-Zustimmung für die Standortverarbeitung erteilt hat.

### Non-Functional Requirements

- **NFR-001**: Performance — Der Matching-Algorithmus MUST Suchergebnisse innerhalb von
  3 Sekunden anzeigen.
- **NFR-002**: Compliance — Das Feature MUST der DSGVO in vollem Umfang entsprechen,
  insbesondere bei Bewegungsdaten; vor dem MVP-Go-Live MUST ein Audit durch den internen
  Datenschutzbeauftragten ohne kritische Befunde (Kategorie 'Blocker' oder 'High')
  abgeschlossen sein.
- **NFR-003**: Portabilität — Das Feature MUST auf iOS 15+ und Android 11+ fehlerfrei
  nutzbar sein.
- **NFR-004**: Security — Alle Feature-Funktionen MUST ausschließlich für Nutzer
  verfügbar sein, die über den unternehmensinternen Identitätsprovider (OAuth2/OIDC)
  authentifiziert sind.

## Edge Cases & Error Handling

- **EC-001**: Wenn der Algorithmus keine Fahrt innerhalb der Toleranzen findet, MUST das
  System 'Keine Fahrten verfügbar' anzeigen und die Aktivierung eines
  Benachrichtigungs-Alarms für diese Route anbieten.
- **EC-002**: Wenn zwei Nutzer simultan den letzten Platz buchen wollen und ein anderer
  schneller war, MUST das System die Fehlermeldung 'Platz bereits vergeben' anzeigen und
  zur Ergebnisliste zurückkehren.
- **EC-003**: Wenn der Fahrer eine bereits gebuchte Fahrt storniert, MUST das System die
  Buchungen der Mitfahrer automatisch stornieren, innerhalb von 1 Minute eine
  Push-Benachrichtigung an die Betroffenen senden und SHOULD alternative Fahrten
  vorschlagen.
- **EC-004**: Wenn die App keinen Zugriff auf das GPS-Modul hat, MUST das System zur
  manuellen Eingabe von Start- und Zieladresse auffordern.
- **EC-005**: Wenn die eingegebene Abfahrtszeit weniger als 15 Minuten in der Zukunft
  liegt, MUST das System die Eingabe zurückweisen und den gültigen Zeitbereich nennen.

## Key Entities

- **Fahrtenangebot**: Vom Fahrer angelegte Fahrt — Fahrer (Nutzerreferenz), Startadresse/
  Geokoordinaten, Zieladresse/Geokoordinaten, Datum und Abfahrtszeit (≥ 15 Minuten in der
  Zukunft), verfügbare Sitzplätze (ganze Zahl 1–4), Status. Beziehung: 1 Fahrtenangebot →
  0..n Buchungen.
- **Buchung**: Verbindliche Reservierung eines Mitfahrers auf einem Fahrtenangebot —
  Mitfahrer (Nutzerreferenz), Fahrtreferenz, Status (gebucht/storniert),
  Statusänderungshistorie mit Zeitstempeln (unveränderlich). Beziehung: n Buchungen →
  1 Fahrtenangebot.
- **Benutzerprofil**: Aus dem Identitätsprovider initialisiertes lokales Profil — Name,
  E-Mail, DSGVO-Zustimmungen (u. a. Standortverarbeitung). Beziehung: 1 Profil → 0..n
  Fahrtenangebote (als Fahrer), 0..n Buchungen (als Mitfahrer).
- **Such-Alarm**: Vom Nutzer aktivierte Benachrichtigung für eine Route ohne aktuelles
  Match — Nutzerreferenz, Start, Ziel, Zeitfenster (Details offen, siehe CL-005).

## Success Criteria

- **SC-001**: Suchergebnisse erscheinen in unter 3 Sekunden.
- **SC-002**: Ein Mitfahrer schließt eine Buchung aus der Detailansicht in maximal
  3 Klicks ab (vorbehaltlich CL-001).
- **SC-003**: Bei Fahrer-Storno erhalten betroffene Mitfahrer die Benachrichtigung
  innerhalb von 1 Minute.
- **SC-004**: 100 % der Buchungs-/Storno-Statusänderungen sind unveränderlich
  protokolliert und für Reports auswertbar.

## Dependencies

- **Upstream Features**: Authentifizierung und Benutzerverwaltung (SRS 3.1) — Login via
  unternehmensinternem Identitätsprovider ist Vorbedingung jeder Feature-Funktion.
- **External Systems**: Unternehmensinterner Identitätsprovider (SSO),
  Push-Benachrichtigungsdienste (iOS/Android), E-Mail-Versand, Kartendienst/Geocoding.
- **Downstream Consumers**: Belohnungssystem (SRS 3.3) und ESG-Reporting (SRS 3.4)
  konsumieren abgeschlossene Fahrten bzw. die protokollierten Buchungsdaten.
- **Constitution Referenz**: Prinzip I (SSO-Pflicht), Prinzip II (DSGVO/
  Datenminimierung, FR-014), Prinzip III (unveränderliche Protokollierung, FR-009),
  Prinzip IV (messbare Nachhaltigkeitswirkung, FR-010).

## Clarification Ledger

| ID | Quellabschnitt | Offene Frage | Blocking | Status |
|----|---------------|--------------|----------|--------|
| CL-001 | SRS 3.2 Akzeptanzkriterien | ⚠️ Widerspruch im Wiki vermerkt: SRS 2026-07-09 nennt "maximal 3 Klicks" zur verbindlichen Buchung, SRS 2026-07-02 "maximal zwei Klicks". FR-004 folgt der neueren Angabe — welche gilt verbindlich? | P1 | Open |
| CL-002 | SRS 3.2 Akzeptanzkriterien | Das aktuelle SRS (2026-07-09) spricht nur von "definierten Systemtoleranzen"; die quantifizierten Werte (3 s, 2 km, 30 Min) stammen aus dem Vorgänger-SRS (2026-07-02). Gelten sie unverändert? | P2 | Open |
| CL-003 | Feature-Zuschnitt | Kein Backlog (1.4.1) im Wiki vorhanden; der Feature-Slug wurde aus SRS 3.2 abgeleitet statt aus einer Backlog-User-Story. Zuschnitt bestätigen. | P3 | Open |
| CL-004 | Interview-Leitfaden Q12 | Ausfallsicherheit bei Fahrer-Storno (Re-Matching-Vorschläge, "Heimfahrt-Garantie") wird nutzerseitig erwartet, ist aber nicht im SRS spezifiziert. In Scope? | P2 | Open |
| CL-005 | SRS 3.2 E1 | Details des Such-Alarms (Benachrichtigungskanal, Gültigkeitsdauer, Auslösekriterien) sind nicht spezifiziert. | P3 | Open |

## Assumptions

- Der Sitzplatzbereich 1–4 aus dem SRS 2026-07-09 gilt; das Vorgänger-SRS nannte keinen
  Wertebereich.
- "Alle betroffenen Parteien" einer Stornierung umfasst den Fahrer und sämtliche
  Mitfahrer der Fahrt.
- Die Toleranzwerte aus dem Vorgänger-SRS (3 s, 2 km, 30 Min) gelten fort, da das
  aktuelle SRS sie nicht ersetzt, sondern nur generisch referenziert (siehe CL-002).
