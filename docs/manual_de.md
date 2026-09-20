# QRadtran – Installations- und Benutzerhandbuch

**Version** 0.3.0　　**Datum** 2026-09-19　　**Herausgeber** Wuhan WaveFront Co., Ltd.

Dieses Handbuch gibt es auch in 简体中文, English, Русский, 日本語, 한국어, Français und Español; nach der Installation liegen alle Fassungen im Ordner `doc\` des Installationsverzeichnisses.

QRadtran ist eine Software zur Berechnung der atmosphärischen Transmission und Strahldichte für Ingenieure im Bereich Infrarot- und elektrooptische Systeme. Unter Windows berechnet sie über eine grafische Oberfläche die spektrale Transmission auf schrägen und horizontalen Pfaden, Pfad- und Himmelsstrahldichte sowie die scheinbare Strahldichte eines Ziels. Sie unterstützt den Vergleich mehrerer Kurven, Parameter-Sweeps zur Erzeugung von Nachschlagetabellen (CSV / HDF5), ein Kommandozeilenwerkzeug und eine Python-Schnittstelle. Der Rechenkern ist das quelloffene libRadtran, das mit der Software installiert wird; eine gesonderte Konfiguration ist nicht nötig.

---

## Inhalt

1. [Systemanforderungen](#1-systemanforderungen)
2. [Installation](#2-installation)
3. [Erster Start](#3-erster-start)
4. [Überblick über die Oberfläche](#4-überblick-über-die-oberfläche)
5. [Parameterfeld im Detail](#5-parameterfeld-im-detail)
6. [Typische Arbeitsabläufe](#6-typische-arbeitsabläufe)
7. [Parameter-Sweeps und Nachschlagetabellen](#7-parameter-sweeps-und-nachschlagetabellen)
8. [Kommandozeilenwerkzeug](#8-kommandozeilenwerkzeug)
9. [Python-Schnittstelle](#9-python-schnittstelle)
10. [Einstellungen](#10-einstellungen)
11. [Rechenverfahren und Genauigkeit](#11-rechenverfahren-und-genauigkeit)
12. [Häufige Fragen](#12-häufige-fragen)
13. [Deinstallation](#13-deinstallation)
14. [Dateien und Ordner](#14-dateien-und-ordner)

---

## 1. Systemanforderungen

| Punkt | Anforderung |
|---|---|
| Betriebssystem | Windows 10 / 11, 64 Bit |
| Prozessor | 4 Kerne oder mehr. Der Kern läuft in mehreren Prozessen parallel; mehr Kerne beschleunigen Batch-Berechnungen |
| Arbeitsspeicher | 8 GB oder mehr |
| Festplatte | Etwa 1,2 GB nach der Installation (einschließlich der Spektraltabellen aller drei Auflösungen). SSD empfohlen, da Berechnungen in feiner Auflösung viele Daten lesen |
| Bildschirm | 1600 × 900 oder größer |
| Sonstiges | Kein Python, MSYS2 oder WSL erforderlich; die VC++-Laufzeit ist enthalten |

Die Python-Schnittstelle ist optional und benötigt ein systemweites Python 3.10 64 Bit mit numpy.

---

## 2. Installation

1. Doppelklicken Sie auf `QRadtran-0.3.0-Setup-x64.exe`.
2. Wählen Sie die Sprache des Installationsprogramms (简体中文, English, Русский, 日本語, 한국어, Français, Deutsch, Español). Die gewählte Sprache wird als Oberflächensprache der Software übernommen; sie lässt sich später unter Werkzeuge → Einstellungen ändern.
3. Lesen Sie die Hinweise zu Lizenzen Dritter und klicken Sie auf Weiter.
4. Wählen Sie den Installationsordner, standardmäßig `C:\Program Files\QRadtran`. Der Pfad darf Umlaute enthalten; ein Netzlaufwerk wird nicht empfohlen.
5. Zusätzliche Aufgaben:
   - **Desktop-Verknüpfung erstellen**: standardmäßig aktiviert.
   - **Installationsordner zum PATH hinzufügen**: standardmäßig deaktiviert. Wenn aktiviert, kann `qradtran-cli` in jeder Eingabeaufforderung aufgerufen werden.
6. Klicken Sie auf Installieren und warten Sie, bis die Dateien kopiert sind (etwa 1 bis 3 Minuten).
7. Auf der letzten Seite können Sie die Software starten oder dieses Handbuch öffnen.

Das Installationsprogramm benötigt Administratorrechte. Um nur für den aktuellen Benutzer zu installieren, wählen Sie in der Rechteabfrage „Nur für mich installieren“; der Standardordner wird dann `%LOCALAPPDATA%\Programs\QRadtran`.

### Portable Nutzung

Das Installationsprogramm ist optional: Kopieren Sie den gesamten Auslieferungsordner `QRadtran\` an einen beliebigen Ort und starten Sie dort `QRadtran.exe`. Alle Abhängigkeiten liegen im Ordner; das Löschen des Ordners deinstalliert die Software.

---

## 3. Erster Start

Sehen Sie nach dem Start auf die Statusleiste unten rechts im Fenster:

```
Kern libradtran 2.0.6 | 7 Prozesse
```

Diese Zeile bedeutet, dass der Rechenkern bereit ist; „Prozesse“ ist die Zahl gleichzeitig laufender Kernprozesse. Steht dort „Kern nicht verfügbar“, öffnen Sie Werkzeuge → Einstellungen und prüfen Sie den Kernpfad, siehe Abschnitt 10.

**Sprache der Oberfläche**: Standardmäßig folgt die Software der Windows-Anzeigesprache und unterstützt vereinfachtes Chinesisch, Englisch, Russisch, Japanisch, Koreanisch, Französisch, Deutsch und Spanisch. Wählen Sie unter Werkzeuge → Einstellungen → Sprache der Oberfläche eine Sprache und starten Sie neu; die Sprache kann auch über die Kommandozeile erzwungen werden, z. B. `QRadtran.exe --lang de`.

Beginnen Sie beim ersten Lauf mit einem voreingestellten Szenario:

1. Menü Datei → Voreingestelltes Szenario laden…, wählen Sie `mls_ground_to_air_3_5um.json` (Atmosphäre mittlere Breiten Sommer, Blick vom Boden auf ein Ziel in 10 km Höhe, 3–5 µm).
2. Klicken Sie in der Symbolleiste auf Ausführen oder drücken Sie F5.
3. Im Feld Aufträge und Protokoll unten erscheint ein Auftrag; ist der Fortschrittsbalken voll, erscheinen die Kurven in der Mitte: Blau ist die Transmission (linke Achse), warme Farben sind die radiometrischen Größen (rechte Achse). Der ganze Lauf dauert etwa 7 Sekunden.

---

## 4. Überblick über die Oberfläche

```
┌─────────────────────────────────────────────────────────────────────┐
│ Datei  Berechnen  Ansicht  Werkzeuge  Hilfe                         │
│ [Neu][Öffnen][Speichern] │ [▶Ausführen][■Stopp][Sweep] │ [Bild][Zoom] │ X-Achse  Dichte │
├──────────────┬───────────────────────────────────────┬──────────────┤
│  Parameter   │  Spektren / Ergebnistabelle           │  Kurven      │
│  ▸ Atmosphäre│                                       │  ☑ Transmiss.│
│  ▸ Geometrie │        (Diagramm mit zwei Achsen)     │  ☑ Pfadstrahl│
│  ▸ Aerosol   │                                       │  Farbe       │
│  ▸ Boden     │                                       │              │
│  ▸ Spektral  │                                       │              │
│  ▸ Ausgaben  │                                       │              │
│  [Berechnen] │  x = 4,35 µm  Transmission 0,812 …    │              │
├──────────────┴───────────────────────────────────────┴──────────────┤
│  Aufträge und Protokoll: Liste (Status / Fortschritt / Dauer) │ Protokoll │
├─────────────────────────────────────────────────────────────────────┤
│ Statusleiste: Cursorwerte                        Kern libradtran 2.0.6 │
└─────────────────────────────────────────────────────────────────────┘
```

Die drei andockbaren Felder lassen sich verschieben, schließen und neu anordnen; ein geschlossenes Feld wird über das Menü Ansicht oder durch Ziehen am Rand wiederhergestellt.

### Symbolleiste

| Schaltfläche | Funktion |
|---|---|
| Neues / Öffnen / Projekt speichern | Ein Projekt ist eine JSON-Datei mit allen Parametern und berechneten Ergebnissen |
| Voreingestelltes Szenario laden | Drei mitgelieferte Beispielszenarien; ihre Namen folgen der Oberflächensprache |
| Ausführen (F5) | Berechnet mit den aktuellen Parametern; das Ergebnis wird dem Diagramm hinzugefügt, ohne frühere zu ersetzen |
| Alle anhalten | Bricht alle laufenden Aufträge ab |
| Parameter-Sweep / LUT | Öffnet den Batch-Dialog, siehe Abschnitt 7 |
| Bild exportieren | Speichert das aktuelle Diagramm als PNG / PDF / JPEG |
| Zoom zurücksetzen (Pos1) | Stellt den vollen Bereich des Diagramms wieder her |
| X-Achse | Einheit der horizontalen Achse: µm / nm / cm⁻¹; wirkt sofort |
| Spektrale Dichte | Einheit der spektralen Dichte radiometrischer Größen: pro µm / pro nm / pro cm⁻¹ |

### Bedienung des Diagramms

| Aktion | Wirkung |
|---|---|
| Mausrad | Zoom um den Cursor |
| Ziehen | Verschieben |
| Doppelklick | Zurücksetzen |
| Maus bewegen | Untere Zeile und Statusleiste zeigen den Wert jeder Kurve am Cursor |
| Kontrollkästchen im Feld Kurven | Kurve ein-/ausblenden |
| Doppelklick auf das Farbfeld | Kurvenfarbe ändern |
| Rechtsklick auf einen Ergebnisnamen | Ergebnis als CSV exportieren oder entfernen |

Die Transmission wird auf der linken Achse (0 bis 1) gezeichnet; Strahldichte und Bestrahlungsstärke auf der rechten Achse, sodass beide überlagert werden können.

---

## 5. Parameterfeld im Detail

### 5.1 Atmosphäre

| Punkt | Beschreibung |
|---|---|
| Standardatmosphäre | Tropisch, mittlere Breiten Sommer, mittlere Breiten Winter, subarktisch Sommer, subarktisch Winter, US-Standard 1976 (die sechs AFGL-Atmosphären) oder eigenes Profil |
| Profil importieren (CSV) | Importiert eine eigene Druck-/Temperatur-/Feuchte-Sondierung, siehe unten |
| Wasserdampfsäule | Wenn aktiviert, wird das gesamte niederschlagbare Wasser auf die angegebenen Millimeter skaliert; sonst gilt der Wert der Standardatmosphäre |
| Ozonsäule | Wie oben, in DU |
| CO₂-Mischungsverhältnis | ppm, Standard 420 |

**Dateiformat des eigenen Profils**: CSV oder durch Leerzeichen getrennter Text mit Kopfzeile. Pflichtspalten: Höhe, Druck, Temperatur und eine Feuchtespalte. Erkannte Spaltennamen (Groß-/Kleinschreibung egal):

| Größe | Spaltennamen | Einheit |
|---|---|---|
| Höhe | z, alt, altitude, height | km (`z_m` für Meter) |
| Druck | p, pres, pressure | hPa (`p_pa` für Pa) |
| Temperatur | t, temp, temperature | K (`t_c` für °C) |
| Feuchte (eine davon) | rh; q; h2o / h2o_vmr | relative Feuchte %; spezifische Feuchte g/kg; Volumenmischungsverhältnis ppmv |
| Ozon (optional) | o3, o3_vmr | ppmv |

Beispiel:

```
z_km,p_hpa,t_k,rh
0.0,1013.0,295.0,70
1.0,900.0,289.0,65
2.0,795.0,283.0,55
5.0,540.0,262.0,40
10.0,265.0,223.0,20
```

Oberhalb des importierten Profils werden die Schichten mit der gewählten Standardatmosphäre ergänzt.

### 5.2 Geometrie

| Pfadtyp | Bedeutung | Parameter |
|---|---|---|
| Schrägpfad (Beobachter → Ziel) | Beobachter und Ziel in unterschiedlicher Höhe | Beobachterhöhe, Zielhöhe, Blickzenitwinkel |
| Horizontaler Pfad | Beobachter und Ziel in gleicher Höhe | Beobachterhöhe, Länge des horizontalen Pfads |
| Gesamte Säule, vertikal | Boden bis Atmosphärenoberrand | Keine |
| Nur Himmelsstrahldichte | Kein Ziel; Strahldichte des Himmels in einer Richtung | Beobachterhöhe, Blickzenitwinkel, Azimut |

Winkelkonventionen:

- **Blickzenitwinkel**: 0° senkrecht nach oben, 90° horizontal, 180° senkrecht nach unten. Er muss unter 90° liegen, wenn das Ziel über dem Beobachter ist, sonst über 90°; andernfalls meldet das Feld einen Fehler.
- **Blickazimut**, **Sonnenazimut**: 0° Nord, im Uhrzeigersinn.
- **Sonnenzenitwinkel**: beeinflusst nur die radiometrischen Größen des Solarbands, nicht die Transmission.
- **Geländehöhe**: Höhe des unteren Endes des Atmosphärenprofils; Beobachter- und Zielhöhe sind Höhen über dem Meeresspiegel.

Bei Zenitwinkeln zwischen 85° und 95° wechselt die Software für nahezu horizontale Pfade zur schichtweisen Extinktionsintegration und gibt eine Genauigkeitswarnung aus.

### 5.3 Aerosol und Wolke

| Punkt | Beschreibung |
|---|---|
| Aerosoltyp | Keine, ländlich, städtisch, maritim, troposphärisch (Shettle-Modelle) oder eine OPAC-Mischung |
| Sichtweite | Horizontale Bodensichtweite in km; legt die Aerosolbeladung der Grenzschicht fest |
| Jahreszeit | Profil Frühling/Sommer oder Herbst/Winter |
| Stratosphärisches Aerosol | Hintergrund / mäßig vulkanisch / stark vulkanisch / extrem vulkanisch |
| Name der OPAC-Mischung | Bei OPAC, z. B. continental_average, maritime_clean, urban, desert |
| Einzelne Wolkenschicht | Wasser- oder Eiswolke mit Basis, Oberkante, Wassergehalt und effektivem Radius |

### 5.4 Boden

| Punkt | Beschreibung |
|---|---|
| Bodenalbedo | Solarband, 0 bis 1 |
| Bodentemperatur | Emissionstemperatur des Bodens im thermischen Infrarot |
| Bodenemissivität | Thermisches Infrarot, 0 bis 1 |

### 5.5 Spektral

| Punkt | Beschreibung |
|---|---|
| Spektrale Einheit | µm / nm / cm⁻¹. Die Grenzen werden beim Einheitenwechsel umgerechnet |
| Von / bis | Spektralbereich. Die Software übergibt Solarband (≤ 5 µm) und thermisches Band (≥ 2,5 µm) getrennt an den Kern und fügt die Ergebnisse zusammen |
| Ausgabeschritt | Abstand des Ausgaberasters; 0 verwendet direkt das Raster repräsentativer Wellenlängen des Kerns |
| Auflösung des Bandmodells | Grob 15 cm⁻¹ / mittel 5 cm⁻¹ / fein 1 cm⁻¹ / LOWTRAN 20 cm⁻¹. Feiner ist genauer und langsamer |
| Löser | DISORT (Mehrfachstreuung, Standard) oder Zweistrom. Wird Strahldichte benötigt, wechselt die Software automatisch zu DISORT |
| DISORT-Ströme | Standard 16; bei stark streuenden Fällen (dichtes Aerosol, Wolke) auf 32 erhöhen |

Richtwerte für die Laufzeit (4-Kern-Büro-PC): 3–5 µm Boden-Luft, grob etwa 7 s, mittel etwa 30 s; 8–12 µm horizontal 1 km etwa 2 s.

### 5.6 Ausgaben

| Punkt | Bedeutung | Einheit |
|---|---|---|
| Pfadtransmission | Spektrale Transmission vom Beobachter zum Ziel | dimensionslos |
| Pfadstrahldichte | Von der Atmosphäre entlang des Pfads emittierte und in die Sichtlinie gestreute Strahldichte | W/(m²·sr·cm⁻¹), umschaltbar |
| Himmelsstrahldichte | Vom Beobachter entlang der Sichtlinie gesehene Himmelsstrahldichte | wie oben |
| Direkte / diffuse abwärts / aufwärts gerichtete Bestrahlungsstärke | Bestrahlungsstärken in Beobachterhöhe | W/(m²·cm⁻¹) |
| Scheinbare Zielstrahldichte | Für gegebene Zieltemperatur und -emissivität die vom Beobachter nach atmosphärischer Dämpfung plus Pfadstrahldichte gesehene Strahldichte; die einfallende Strahldichte am Ziel wird ebenfalls ausgegeben | W/(m²·sr·cm⁻¹) |

---

## 6. Typische Arbeitsabläufe

### 6.1 Einzelberechnung und Vergleich

1. Parameter einstellen und auf Ausführen klicken.
2. Ein oder zwei Parameter ändern (z. B. Sichtweite von 23 km auf 5 km) und erneut ausführen. Das neue Ergebnis wird demselben Diagramm in anderer Farbe hinzugefügt, mit dem Szenarionamen in der Legende.
3. Im Feld Kurven rechts die zu vergleichenden Kurven ein- oder ausblenden.
4. Auf die Registerkarte Ergebnistabelle wechseln, um die Zahlen zu lesen; Achsen- und Dichteeinheit folgen der Symbolleiste.

### 6.2 Speichern und Wiederherstellen

Datei → Projekt speichern legt die aktuellen Parameter und alle Ergebnisse in einer JSON-Datei ab. Beim späteren Öffnen werden die Kurven ohne Neuberechnung wiederhergestellt.

### 6.3 Export

- Diagramm: Datei → Bild exportieren oder die Schaltfläche in der Symbolleiste, PNG / PDF / JPEG.
- Daten: Rechtsklick auf ein Ergebnis im Feld Kurven → Dieses Ergebnis als CSV exportieren. Die CSV verwendet die aktuell in der Symbolleiste gewählten Achsen- und Dichteeinheiten; die Kopfzeile nennt die Einheiten.

### 6.4 Szenario Zielerfassung

Um die scheinbare Strahldichte eines Ziels am Detektor zu erhalten:

1. In Geometrie Schrägpfad wählen, Beobachter- und Zielhöhe sowie Zenitwinkel eingeben.
2. Unter Ausgaben Scheinbare Zielstrahldichte aktivieren, Zieltemperatur und -emissivität eingeben.
3. Der Lauf liefert vier Kurven: Transmission, Pfadstrahldichte, scheinbare Zielstrahldichte und einfallende Strahldichte am Ziel.
4. Ziehen Sie das Ergebnis Nur Himmelsstrahldichte derselben Geometrie von der scheinbaren Zielstrahldichte ab, um den Strahldichtekontrast Ziel/Hintergrund zu erhalten.

---

## 7. Parameter-Sweeps und Nachschlagetabellen

Berechnen → Parameter-Sweep / LUT… geht vom aktuellen Parameterfeld aus, berechnet jede Kombination (kartesisches Produkt) der gewählten Parameterwerte und schreibt eine Nachschlagetabelle.

### Dialogelemente

| Punkt | Beschreibung |
|---|---|
| Sweep-Dimensionen | Ein Parameter je Zeile. Parameter werden als JSON-Zeiger geschrieben; die Auswahlliste nennt die gängigen. Werte sind eine Liste `1, 2, 5, 10` oder ein Bereich `0:10:2` (Start:Ende:Schritt); Zeichenkettenparameter werden mit Namen geschrieben, z. B. `Rural, Urban` |
| Spalte | Koordinatenname dieser Dimension in der Ausgabedatei, Standard ist der Parametername |
| Ausgaben | Ist nichts gewählt, werden alle Größen des Szenarios geschrieben |
| Bandmittelwerte | Optional; Bandmittel und Bandintegral über µm-Bänder, zusätzlich geschrieben |
| Ausgabedatei | Die Endung bestimmt das Format: `.h5` für HDF5, `.csv` für eine lange Tabelle |
| Fortsetzen | Überspringt bereits berechnete Kombinationen beim erneuten Start nach einer Unterbrechung |

Der Dialog zeigt die Gesamtzahl der Kombinationen und die Zahl der Kernläufe je Kombination. Der Lauf kann abgebrochen werden; fertige Teile bleiben erhalten.

### Aufbau der HDF5-Datei

```
/<Größe>                      [dim1, …, dimN, spektral]   Daten, Einheit im Attribut "units"
/bands/<Größe>_average        [dim1, …, dimN, Band]       Bandmittel
/bands/<Größe>_integral       [dim1, …, dimN, Band]       Bandintegral
/coords/wavenumber            [spektral]                  cm⁻¹
/coords/wavelength_um         [spektral]                  µm
/coords/<Spalte>              [Werte]                     Koordinate jeder Dimension (Zahl oder Zeichenkette)
/coords/band                  [Band]                      Bandnamen
/filled                       [dim1, …, dimN]             1 = Kombination berechnet
Wurzelattribute: kernel, kernel_version, created_utc, base_scenario (JSON des Basisszenarios), plan (JSON des Sweep-Plans)
```

Lesen in Python (benötigt h5py):

```python
import h5py
with h5py.File("lut.h5") as f:
    T = f["Transmittance"][...]            # Form [dim1, …, spektral]
    nu = f["coords/wavenumber"][...]
    tgt = f["coords/target_km"][...]
```

`pyqradtran\read_lut_h5.py` im Installationsordner ist ein fertiges Anzeigeskript.

### Lange CSV-Tabelle

Eine Zeile je (Dimensionswerte…, Wellenzahl, Größe, Wert); Bandmittelwerte landen in `<Datei>.bands.csv`. Geeignet für Excel-Pivottabellen oder pandas.

---

## 8. Kommandozeilenwerkzeug

`qradtran-cli.exe` liegt im Installationsordner; mit aktiviertem „Zum PATH hinzufügen“ funktioniert es von überall.

```
qradtran-cli example --out case.json         Vorlage einer Szenariodatei schreiben
qradtran-cli validate case.json              Parameter prüfen
qradtran-cli plan case.json                  geplante Kernläufe auflisten, ohne Berechnung
qradtran-cli run case.json --out out.csv     berechnen und CSV schreiben (--unit um|nm|cm-1  --density cm-1|nm|um)
qradtran-cli run case.json --out out.json    JSON schreiben (mit Metadaten)
qradtran-cli batch-example --out plan.json   Vorlage eines Sweep-Plans schreiben
qradtran-cli batch plan.json --out lut.h5    Batch-Berechnung (.h5 oder .csv; --resume zum Fortsetzen)
qradtran-cli kernel-info                     Kernstatus und verfügbare Auflösungen anzeigen
```

Rückgabecodes: 0 Erfolg, 1 Argumentfehler, 2 Validierung fehlgeschlagen, 3 Kernfehler. Szenariodateien und GUI-Projektdateien sind austauschbar.

---

## 9. Python-Schnittstelle

`pyqradtran\` im Installationsordner ist ein Python-Paket, gebaut für Python 3.10 64 Bit. Fügen Sie den Installationsordner vor der Nutzung zum `PYTHONPATH` hinzu:

```bat
set PYTHONPATH=C:\Program Files\QRadtran
python
```

```python
import pyqradtran as qr

s = qr.Scenario.standard("MidlatitudeSummer")
s.geometry.mode = "SlantPath"
s.geometry.observer_alt_km = 0.0
s.geometry.target_alt_km = 10.0
s.geometry.view_zenith_deg = 30.0
s.aerosol.type = "Rural"
s.aerosol.visibility_km = 23.0
s.spectral.set_range(3, 5, unit="Micrometer", step=0.01, resolution="Coarse")
s.outputs.transmittance = True
s.outputs.path_radiance = True

r = qr.run(s)
nu = r.wavenumber                   # numpy-Array, cm⁻¹
T = r["Transmittance"]
L = r.series("PathRadiance", per="Micrometer")     # umgerechnet auf pro µm
print(r.band_average("Transmittance", 3.0, 5.0))  # {'average':…, 'integral':…}
r.to_csv("out.csv", unit="Micrometer")

# Parameter-Sweep
plan = qr.BatchPlan()
plan.base = s
plan.add_dimension("/geometry/target_alt_km", [1, 2, 5, 10], "tgt_km")
plan.add_band("MWIR", 3.0, 5.0)
qr.batch(plan, "lut.h5")
```

Das vollständige Beispiel ist `pyqradtran\python_quickstart.py`. Die Bindungen teilen Kernbibliothek und Kern mit der GUI, die Ergebnisse sind identisch.

---

## 10. Einstellungen

Werkzeuge → Einstellungen:

| Punkt | Beschreibung |
|---|---|
| uvspec.exe / libRadtran-Datenverzeichnis | Ort des Kerns, standardmäßig `kernels\libradtran` im Installationsordner. Normalerweise keine Änderung nötig |
| Kern erkennen | Zeigt Kernversion und installierte Auflösungen |
| Max. gleichzeitige Kernprozesse | 0 = automatisch (CPU-Kerne minus 1). Bei knappem Speicher verringern |
| Zeitlimit je Kernlauf | Sekunden. Feine Auflösung, breite Bänder oder viele Ströme können einen höheren Wert erfordern |
| Arbeitsverzeichnis bei Fehler behalten | Behält bei einem Fehlschlag die Ein- und Ausgabedateien des Kerns zur Diagnose; der Ort steht im Protokoll |
| Arbeitsverzeichnis auch bei Erfolg behalten | Nur zur Diagnose, normalerweise aus |
| Sprache der Oberfläche | Dem System folgen oder eine der acht Sprachen festlegen; wirkt nach Neustart |

Arbeitsverzeichnisse werden unter `%LOCALAPPDATA%\WaveFront\QRadtran\runs\` angelegt.

---

## 11. Rechenverfahren und Genauigkeit

- **Kern**: uvspec aus libRadtran 2.0.6, als eigener Prozess ausgeführt; molekulare Absorption mit dem REPTRAN-Bandmodell repräsentativer Wellenlängen (basierend auf HITRAN 2004 und später), Mehrfachstreuung mit DISORT.
- **Schrägpfad-Transmission**: im Solarband aus dem Verhältnis der direkten Bestrahlungsstärken in beiden Höhen; im thermischen Band aus dem Verhältnis der Strahldichtedifferenzen zweier Läufe mit gestörter Bodentemperatur.
- **Horizontale und nahezu horizontale Pfade**: Der Extinktionskoeffizient wird aus der vertikalen Transmission der Schicht abgeleitet und entlang des sphärischen Pfads integriert.
- **Pfadstrahldichte**: Strahldichte am Beobachter minus Strahldichte am Ziel mal Transmission.
- **Genauigkeit**: Vergleich mit MODTRAN4 in den langwelligen und mittelwelligen atmosphärischen Fenstern; in 6 von 8 Fällen weicht die bandgemittelte Fenstertransmission um weniger als 0,04 ab, im besten Fall um 0,003. Innerhalb starker Absorptionsbanden unterscheiden sich die beiden Generationen spektroskopischer Daten systematisch. Siehe den mitgelieferten Validierungsbericht.
- **REPTRAN-Auflösungen**: fein 1 cm⁻¹ entspricht der Klasse des 1-cm⁻¹-Bandmodells von MODTRAN; grob 15 cm⁻¹ eignet sich für schnelle Abschätzungen.

---

## 12. Häufige Fragen

**Die Statusleiste zeigt nach dem Start „Kern nicht verfügbar“.**
Der Installationsordner wurde verschoben oder `kernels\libradtran` wurde von einem Virenscanner entfernt. Pfade in den Einstellungen prüfen; bei Bedarf neu installieren.

**Die Schaltfläche Ausführen ist ausgegraut.**
Unter dem Parameterfeld steht eine rote Validierungsmeldung; beheben Sie das Genannte. Häufige Ursachen: Schrägpfad gewählt, aber Zielhöhe gleich Beobachterhöhe; Zenitwinkel widerspricht der Lage des Ziels oben/unten; identische Spektralgrenzen.

**Der Lauf scheitert, im Protokoll steht „Netcdf error … reptran_…_medium“.**
Die Datentabellen der gewählten Auflösung fehlen. Das vollständige Installationsprogramm enthält alle drei; bei einem reduzierten Paket Grob verwenden oder die Daten nachinstallieren.

**Die Transmission im thermischen Infrarot ist überall null.**
Trat in Versionen vor 0.2.0 mit dem Zweistromlöser auf; behoben – DISORT wird automatisch verwendet, wenn Strahldichte benötigt wird.

**Die Berechnung ist langsam.**
Feine Auflösung mit breitem Band und 32 Strömen ist am langsamsten. Parameter mit grober Auflösung abstimmen und das endgültige Diagramm mit feiner Auflösung erzeugen; bei Batches alle verfügbaren Prozesse nutzen.

**Die Werte der rechten Achse sind schwer zu deuten.**
Die rechte Achse zeigt radiometrische Größen; die Einheit folgt der Einstellung Spektrale Dichte in der Symbolleiste: pro cm⁻¹, pro nm oder pro µm. Tabellen und CSV verwenden dieselbe Einstellung.

**Die Oberfläche ist englisch.**
Ist die Windows-Anzeigesprache nicht unter den unterstützten, fällt die Software auf Englisch zurück. Sprache unter Werkzeuge → Einstellungen → Sprache der Oberfläche wählen und neu starten.

**Python meldet „DLL load failed“.**
`PYTHONPATH` muss auf den Installationsordner selbst zeigen (die Ebene mit `qradtran_core.dll` und `Qt6Core.dll`), nicht auf den Unterordner `pyqradtran`; Python muss 3.10 64 Bit sein.

**Kann ich mein eigenes libRadtran verwenden?**
Ja. Richten Sie uvspec.exe und das Datenverzeichnis in den Einstellungen auf Ihre Version; sie muss 2.0.x sein.

---

## 13. Deinstallation

Deinstallieren Sie QRadtran über Einstellungen → Apps oder starten Sie QRadtran deinstallieren im Startmenü. Die Deinstallation belässt die Projektdateien des Benutzers und die Laufaufzeichnungen unter `%LOCALAPPDATA%\WaveFront\QRadtran`; löschen Sie sie bei Bedarf von Hand.

Bei der portablen Variante genügt das Löschen des Ordners.

---

## 14. Dateien und Ordner

```
QRadtran\
├── QRadtran.exe              grafische Oberfläche
├── qradtran-cli.exe          Kommandozeile
├── qradtran_core.dll         Kernbibliothek
├── Qt6*.dll, platforms\, styles\, translations\ …   Qt-Laufzeit
├── hdf5.dll, msvcp140.dll, vcruntime140*.dll        HDF5 und VC++-Laufzeit
├── kernels\libradtran\
│   ├── bin\uvspec.exe        Rechenkern (MinGW-Build) und seine Laufzeit
│   ├── data\                 Atmosphärenprofile, Aerosol, Wolken, Sonnenspektren und REPTRAN-Tabellen
│   ├── source\               libRadtran-Quellarchiv, Patches und Build-Skript (GPL-Anforderung)
│   └── VERSION
├── data\presets\             Beispielszenarien
├── pyqradtran\               Python-Bindungen und Beispiele
├── licenses\                 Lizenztexte Dritter
└── doc\                      dieses Handbuch (md und html in acht Sprachen)
```

Projektdateien (`.json`) sowie exportierte CSV / HDF5 / Bilder werden am vom Benutzer gewählten Ort gespeichert.

---

## Support

Wuhan WaveFront Co., Ltd.　　WeChat: angaio
