# Prüfung -- Mobile App zur Dokumentation elektrischer Prüfungen

Android-App auf Basis von **MIT App Inventor** zur mobilen Erfassung und
Übertragung von Prüfdaten für ortsfeste Bauteile sowie RCD-Prüfungen.

Die Anwendung stellt mehrere Prüfbereiche bereit, unterstützt die
Identifikation über QR-/Barcodes und überträgt die erfassten Datensätze
per HTTP an einen konfigurierbaren Endpunkt, beispielsweise einen
HTTP-Trigger in Microsoft Power Automate.

## Funktionen

### 1. Prüfung ortsfester Bauteile

Erfassung von Stammdaten und Prüfergebnissen:

-   Leitungsart, Querschnitt und Aderanzahl
-   Ort, Sicherung und Gerätenummer
-   Gerätenummer per QR-/Barcode-Scanner erfassen
-   Sichtkontrolle:
    -   Brandschottung
    -   Leitungsverlegung
    -   Schutzleiteranschluss
    -   richtiger Querschnitt
    -   Berührungsschutz
-   Spannungsprüfung:
    -   L1--L2, L2--L3, L1--L3
    -   L1--N, L2--N, L3--N
    -   N--PE
    -   24-V-Kontrolle
-   Widerstandsmessungen:
    -   ZS, RE, IK und PE/Gehäuse
    -   RISO L1--PE, L2--PE und L3--PE
-   RCD-Status, Drehfeld und Funktionsprüfung
-   Datum/Uhrzeit und Prüfer

### 2. RCD-Messung

Separater Prüfbereich mit folgenden Eingabefeldern:

-   RCD-Nummer (auch per Scan)
-   Berührungsspannung U
-   Erdungswiderstand RE
-   Auslösezeit tA
-   RCD-Status
-   Datum/Uhrzeit und Prüfer

### 3. RCD-Auslösung

Dokumentation einer RCD-Auslösung mit:

-   RCD-Nummer (auch per Scan)
-   Status „RCD ok"
-   Datum/Uhrzeit
-   Prüfer

### 4. Einstellungen

-   Power-Automate-URL hinterlegen
-   URL lokal speichern
-   URL löschen
-   QR-Code zur konfigurierten URL anzeigen
-   URL über den Scanner erfassen

## Technischer Aufbau

  -----------------------------------------------------------------------
  Bestandteil                         Verwendung
  ----------------------------------- -----------------------------------
  MIT App Inventor                    Entwicklung der Android-App

  TinyDB                              Lokale Speicherung von
                                      Einstellungen und Prüfdaten

  BarcodeScanner                      QR-/Barcode-Erfassung

  Web-Komponente                      HTTP-Kommunikation

  Clock                               Datum und Uhrzeit

  WebViewer                           Anzeige des QR-Codes

  Power Automate (optional)           Annahme und Weiterverarbeitung der
                                      Prüfdaten
  -----------------------------------------------------------------------

### Datenfluss

``` text
Android-App
    |
    | Prüfdaten als JSON (HTTP POST)
    v
Konfigurierter HTTP-Endpunkt
    |
    +--> Microsoft Power Automate (optional)
              |
              +--> Weitere Verarbeitung / Ablage
```

Die Ziel-URL wird in der App unter **Einstellungen** konfiguriert. Für
die HTTP-Anfrage wird `Content-Type: application/json` gesetzt.

## Datenformat

Die App unterscheidet die Datensätze über das Feld `Prüfung`.

Beispielhafte Feldstruktur der ortsfesten Prüfung:

``` json
{
  "Prüfung": "ortsfest",
  "Leitungsart": "NYM",
  "Querschnitt": "2.5",
  "Ader": "5",
  "Gerät": "Anlagenkennung",
  "Ort": "Halle 1",
  "Sicherung": "Feld / Sicherung",
  "Leitungsverlegung": true,
  "Brandschottung": true,
  "Schutzleiter": true,
  "Querschnitt_ok": true,
  "Berührungsschutz": true,
  "L1_L2": true,
  "L2_L3": true,
  "L1_L3": true,
  "L1_N": true,
  "L2_N": true,
  "L3_N": true,
  "N_PE": true,
  "Kleinspg_ok": true,
  "ZS": "Messwert",
  "RE": "Messwert",
  "IK": "Messwert",
  "PE": "Messwert",
  "RISO1": "Messwert",
  "RISO2": "Messwert",
  "RISO3": "Messwert",
  "RCD_ok": true,
  "Funktion_ok": true,
  "Drehfeld": "Drehfeld rechts",
  "Datum": "18.11.2026 18:00:00",
  "Prüfer": "Prüfername"
}
```

Weitere Prüfarten verwenden diese Kennungen:

  Prüfbereich                 Wert im Feld `Prüfung`
  --------------------------- ------------------------
  Ortsfeste Prüfung           `ortsfest`
  RCD-Messung                 `rcd`
  RCD-Auslösung / Prüftaste   `rcd_taste`

Die Beispielwerte dienen nur zur Veranschaulichung. Maßgeblich sind die
tatsächlich in der App erfassten Werte und die implementierte
Feldstruktur.

## Projekt öffnen und bearbeiten

1.  Repository herunterladen oder klonen.
2.  MIT App Inventor öffnen: <https://ai2.appinventor.mit.edu/>
3.  Im Menü **Projects → Import project (.aia) from my computer**
    auswählen.
4.  Die Projektdatei `md_pruefung.aia` importieren.
5.  Die Power-Automate-URL unter **Einstellungen** eintragen.
6.  App auf einem Android-Gerät testen.

## Voraussetzungen

-   MIT App Inventor
-   Android-Gerät mit Kamera für QR-/Barcode-Scans
-   Erreichbarer HTTP-Endpunkt
-   Netzwerkverbindung zum konfigurierten Endpunkt für die Übertragung

Die Projektkonfiguration verwendet Android **minSdk 24**.

## Repository-Struktur

``` text
.
├── md_pruefung.aia       # MIT-App-Inventor-Projekt zum Import
├── README.md             # Diese Dokumentation
└── assets/
    └── logo_emsland-group.png
```

Die `.aia`-Datei ist ein ZIP-basiertes MIT-App-Inventor-Projektarchiv.
Die interne Projektstruktur enthält die vier Screens `Screen1` bis
`Screen4`.

## Hinweise und Grenzen

-   Die App dokumentiert Eingaben und Prüfergebnisse. Sie ersetzt keine
    fachgerechte Prüfung durch eine qualifizierte Person.
-   Die Einhaltung der jeweils geltenden Normen, Grenzwerte und
    Prüfverfahren muss durch den Prüfverantwortlichen sichergestellt
    werden.
-   Eine automatische fachliche Bewertung aller Messwerte oder eine
    normbasierte Freigabe ist durch die hier dokumentierte
    Projektstruktur nicht nachgewiesen.
-   Die lokale Speicherung über TinyDB ist Bestandteil der App. Ein
    belastbares Offline-Synchronisations- und Wiederholungsmechanismus
    sollte vor dem produktiven Einsatz gesondert getestet werden.
-   Die Power-Automate-URL ist eine vertrauliche Konfiguration. Sie
    sollte nicht in öffentliche Screenshots, Quelltextbeispiele oder
    Commits aufgenommen werden.
-   Vor einem produktiven Einsatz sind HTTP-Fehlerfälle, Berechtigungen,
    Datenvalidierung, Dublettenvermeidung und die dauerhafte Ablage der
    Prüfdaten zu testen.

## Version

-   App-Name: `md_pruefung`
-   Anzeigename: `Prüfung`
-   Version: `2026.11.18.01`
-   Versioncode: `154`
-   Android minSdk: `24`
-   Entwicklungsplattform: MIT App Inventor
