# Prüfung

**Mobile Android-App zur Erfassung und Übertragung von Prüfergebnissen
für ortsfeste Bauteile und RCDs.**

Das Projekt wurde mit **MIT App Inventor** erstellt. Die App bietet
getrennte Eingabemasken für die ortsfeste Prüfung, die RCD-Messung und
die RCD-Auslösung. Prüfdaten können in der App eingetragen und per HTTP
an eine konfigurierte Schnittstelle übertragen werden.

## Funktionsumfang

### Ortsfeste Prüfung

Der Startbildschirm `Screen1` enthält die Erfassung folgender Angaben:

**Stammdaten** - Leitungsart: `NYM`, `H07RN-F`, `YSLY-JZ` oder `NYY` -
Querschnitt und Aderanzahl - Ort und Sicherung - Gerätenummer mit
Scan-Funktion - Prüfer sowie Datum und Uhrzeit

**Sichtkontrolle** - Brandschottung - Leitungsverlegung - Anschluss
Schutzleiter - Richtiger Querschnitt - Berührungsschutz

**Spannungsmessungen / Kontrollen** - L1--L2, L2--L3 und L1--L3 - L1--N,
L2--N und L3--N - N--PE - 24-V-Kontrolle

**Widerstandsmessungen** - ZS - RE - IK - PE/Gehäuse - RISO L1--PE -
RISO L2--PE - RISO L3--PE

**Weitere Prüfergebnisse** - RCD OK - Funktion OK - Drehfeld rechts oder
links

Über die Schaltflächen kann zu den RCD-Prüfseiten gewechselt und eine
Synchronisierung gestartet werden.

### RCD-Messung

Der Bildschirm `Screen3` erfasst:

-   RCD-Nummer (mit Scan-Funktion)
-   RCD-Status
-   Berührungsspannung `U`
-   Erdungswiderstand `RE`
-   Auslösezeit `tA`
-   Prüfer
-   Datum und Uhrzeit

### RCD-Auslösung

Der Bildschirm `Screen4` dient zur Dokumentation der RCD-Auslösung:

-   RCD-Nummer (mit Scan-Funktion)
-   RCD-Status
-   Prüfer
-   Datum und Uhrzeit

### Einstellungen

Der Bildschirm `Screen2` bietet:

-   Eingabe der Power-Automate-URL
-   Speichern und Löschen der URL
-   QR-Code-Anzeige für die URL
-   Einlesen einer URL per Scan
-   Status-/Datenanzeige

## Datenübertragung

Die App verwendet die MIT-App-Inventor-Komponente **Web** für
HTTP-Anfragen. Als Request-Header wird `Content-Type: application/json`
gesetzt. Die Zieladresse wird über die Einstellungen bereitgestellt.

Die App enthält außerdem lokale `TinyDB`-Komponenten. Die Schaltflächen
**eintragen** und **synch** sind für die Erfassung bzw. Übertragung der
Datensätze vorgesehen.

``` text
Android-App (MIT App Inventor)
        |
        | JSON / HTTP
        v
Konfigurierte URL
        |
        v
Power Automate HTTP-Trigger
        |
        v
Weiterverarbeitung und Ablage
```

Power Automate ist ein möglicher Empfänger der HTTP-Anfrage. Der
konkrete Flow und dessen Zielsystem müssen separat eingerichtet werden.

## JSON-Datensatztypen

Die App kennzeichnet die Prüfungsart im JSON-Feld `Prüfung`:

  Prüfungsart         Kennung
  ------------------- -------------
  Ortsfeste Prüfung   `ortsfest`
  RCD-Messung         `rcd`
  RCD-Auslösung       `rcd_taste`

### Beispiel: ortsfeste Prüfung

Die folgenden Felder orientieren sich an den im App-Projekt
zusammengesetzten Daten. Werte sind beispielhaft:

``` json
{
  "Prüfung": "ortsfest",
  "Leitungsart": "NYM",
  "Querschnitt": "2.5",
  "Ader": "5",
  "Gerät": "Anlagenkennung",
  "Ort": "Halle 1",
  "Sicherung": "Feld / Sicherung",
  "Leitungsverlegung": "true",
  "Brandschottung": "true",
  "Schutzleiter": "true",
  "Querschnitt_ok": "true",
  "Berührungsschutz": "true",
  "L1_L2": "true",
  "L2_L3": "true",
  "L1_L3": "true",
  "L1_N": "true",
  "L2_N": "true",
  "L3_N": "true",
  "N_PE": "true",
  "Kleinspg_ok": "true",
  "ZS": "Messwert",
  "RE": "Messwert",
  "IK": "Messwert",
  "PE": "Messwert",
  "RISO1": "Messwert",
  "RISO2": "Messwert",
  "RISO3": "Messwert",
  "RCD_ok": "true",
  "Funktion_ok": "true",
  "Drehfeld": "Drehfeld rechts",
  "Datum": "18.11.2026 18:00:00",
  "Prüfer": "Prüfername"
}
```

### Beispiel: RCD-Messung

``` json
{
  "Prüfung": "rcd",
  "UIN": "Messwert",
  "RE": "Messwert",
  "tA": "Messwert",
  "RCD_Nr": "RCD-001",
  "RCD_ok": "true",
  "Datum": "18.11.2026 18:00:00",
  "Prüfer": "Prüfername"
}
```

### Beispiel: RCD-Auslösung

``` json
{
  "Prüfung": "rcd_taste",
  "RCD_Nr": "RCD-001",
  "RCD_ok": "true",
  "Datum": "18.11.2026 18:00:00",
  "Prüfer": "Prüfername"
}
```

**Hinweis:** Die Checkbox-Werte werden im Projekt als Textwerte
(`"true"` / `"false"`) in die JSON-Zeichenfolge eingefügt. Empfänger
sollten daher nicht ungeprüft JSON-Boolean-Werte erwarten. Die
tatsächliche Nutzlast sollte beim Einrichten des Flows mit einem
Testlauf kontrolliert werden.

## Projekt importieren

1.  [MIT App Inventor](https://ai2.appinventor.mit.edu/) öffnen.
2.  **Projects → Import project (.aia) from my computer** auswählen.
3.  Die Datei `md_pruefung(1).aia` bzw. die im Repository abgelegte
    `.aia`-Projektdatei auswählen.
4.  Projekt öffnen und die Komponenten sowie Blöcke bearbeiten.
5.  Unter **Einstellungen** die URL des HTTP-Endpunkts eintragen.
6.  App auf einem Android-Gerät testen.

## Voraussetzungen

-   MIT App Inventor
-   Android-Gerät
-   Kamera-Berechtigung bzw. Scanner-Unterstützung für
    QR-/Barcode-Erfassung
-   Netzwerkzugriff auf den konfigurierten HTTP-Endpunkt
-   Optional: Microsoft Power Automate mit eingerichtetem HTTP-Trigger

Die Projektdatei definiert `AndroidMinSdk: 24` und ein responsives
Layout.

## Projektstruktur

``` text
Repository/
├── README.md
├── md_pruefung(1).aia
└── assets/
    └── logo_emsland-group.png
```

Die `.aia`-Datei ist das MIT-App-Inventor-Projektarchiv. Sie enthält die
vier Screens, deren Designer-Oberflächen und die zugehörigen
Programmblöcke sowie das Logo.

## Technische Komponenten

  Komponente       Einsatz
  ---------------- -------------------------
  TinyDB           Lokale Datenspeicherung
  BarcodeScanner   QR-/Barcode-Scan
  Web              HTTP-Anfragen
  Clock            Datum und Uhrzeit
  WebViewer        QR-Code-Anzeige

## Hinweise

-   Die App dient der Erfassung und Dokumentation von Prüfdaten. Sie
    ersetzt keine fachgerechte Prüfung und keine Bewertung durch eine
    qualifizierte Elektrofachkraft.
-   Prüfverfahren, Grenzwerte und die Beurteilung der Messwerte müssen
    anhand der geltenden Vorschriften und der konkreten Anlage
    festgelegt werden.
-   Eine automatische normbasierte Bewertung der Messwerte ist aus dem
    vorliegenden Projekt nicht ersichtlich.
-   Vor dem produktiven Einsatz sind die HTTP-Übertragung,
    Fehlerbehandlung, lokale Speicherung, Synchronisierung und das
    Verhalten bei fehlender Netzwerkverbindung praktisch zu testen.
-   Die Power-Automate-URL kann Zugriff ermöglichen. Behandle sie daher
    vertraulich und veröffentliche sie nicht in Screenshots oder
    Beispieldaten.
-   Die Beispiele in dieser README sind Dokumentationsbeispiele und
    keine vollständigen Prüf- oder Freigabeprotokolle.

## Projektinformationen

  Eigenschaft            Wert
  ---------------------- -------------------------
  App-Name               Prüfung
  Projektname            `md_pruefung`
  Entwicklungsumgebung   MIT App Inventor
  Screens                `Screen1` bis `Screen4`
  Version                `2026.11.18.02`
  Versioncode            `155`
  Android-Minimum        API 24
