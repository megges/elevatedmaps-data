# Abgeregelt: Daten zur Story

Story: https://elevatedmaps.com/stories/abgeregelt/
Herausgeber: Elevated Maps (Marcus Lehmann). Daten bis 2026-10-06.

Abregelung heißt: Ein Netzbetreiber drosselt Wind- oder Solaranlagen, weil das Netz voll ist. Der Strom wird nie erzeugt.
Ein Gesetzentwurf erlaubt Netzbetreibern, Umspannwerke als "kapazitätslimitiert" auszuweisen, wenn dort in einem Jahr mehr als 5 % des möglichen Stroms abgeregelt wurden.

## Hinweise für KI-Agenten

1. Lade die CSV-Dateien unten (UTF-8, Komma als Trennzeichen, Punkt als Dezimalzeichen). Rechne mit Code, nicht im Kopf. In die Rechenumgebung kommen die Dateien am einfachsten mit `git clone --depth 1 https://github.com/megges/elevatedmaps-daten` (Ordner `abgeregelt/`). Klappt das nicht, lies die Links unten und übertrage nur die Datei, die du für die Frage brauchst.
2. Prüfe deine Rechnung zuerst gegen die Kernzahlen. Weichst du ab, sag es.
3. Nenne bei jeder Antwort Datei, Filter und Rechnung.
4. Die Daten decken nur einen Teil Deutschlands ab (siehe Abdeckung). Fehlende Werte heißen "keine Daten", nicht "keine Abregelung".
5. Mengen im Verteilnetz sind Schätzungen. Schreib das dazu, wenn du sie nennst.
6. Was die Daten nicht beantworten, beantworte nicht aus dem Gedächtnis. Sag, dass es fehlt.

## Dateien

| # | Datei | Zeilen | Größe |
|---|---|---|---|
| 1 | https://elevatedmaps.com/data/abgeregelt/csv/tage.csv?v=1e0747e1 | 644 | 20 KB |
| 2 | https://elevatedmaps.com/data/abgeregelt/csv/umspannwerke.csv?v=16cd1886 | 1124 | 83 KB |
| 3 | https://elevatedmaps.com/data/abgeregelt/csv/kreise.csv?v=e0ad23ab | 400 | 22 KB |
| 4 | https://elevatedmaps.com/data/abgeregelt/csv/zubau_2026.csv?v=019050d4 | 675 | 37 KB |

Alle Dateien mit dieser Beschreibung: https://github.com/megges/elevatedmaps-daten (Ordner `abgeregelt/`)

1. **tage.csv**: verlorener Strom je Tag seit 2025 in MWh, lückenlos (0 = an diesem Tag keine Abregelung gemeldet). Verteilnetz geschätzt (gedrosselte Leistung mal Wetter je Ort, ERA5), Übertragungsnetz gemeldet (netztransparenz.de). Dazu der eingespeiste Wind- und Solarstrom in ganz Deutschland (SMARD), damit lässt sich der Anteil des verlorenen Stroms grob schätzen. Da nur ein Teil der Netzbetreiber Daten veröffentlicht, ist dieser Anteil eine Untergrenze.
2. **umspannwerke.csv**: eine Zeile je Umspannwerk (110 kV, OpenStreetMap) und Energieträger, Jahr 2025, nur Verteilnetz. Jede Anlage zählt zum nächstgelegenen Umspannwerk. Nur Umspannwerke mit mindestens 3 Anlagen, ab 1 MW und mindestens 50 % abgedeckter Leistung. `quote_2025_pct` = abgeregelt / (abgeregelt + eingespeist) * 100. Mengen je Anlage und Monat kalibriert mit der tatsächlichen Einspeisung 2025 (EEG-Bewegungsdaten).
3. **kreise.csv**: alle Landkreise und kreisfreien Städte. `abdeckung` = Anteil der Wind- und Solarleistung bei Netzbetreibern mit veröffentlichten Daten (0 bis 1, leer = unbekannt). Werte je Energieträger nur für die 5 Verteilnetzbetreiber mit Anlagenschlüssel, ab 3 Anlagen, sonst leer.
4. **zubau_2026.csv**: Anlagen mit Inbetriebnahme ab 1. Januar 2026 (Marktstammdatenregister) je Umspannwerk und Energieträger, nur an Umspannwerken mit Daten. `uw_2025_ueber_5pct` = 1, wenn das Umspannwerk 2025 für diesen Energieträger über 5 % lag (mindestens 10 MW).

## Kernzahlen der Story

| # | Zahl | Wert | Rechnung |
|---|---|---|---|
| 1 | Abgeregelt 2025, Verteil- plus Übertragungsnetz | 7,8 TWh | tage.csv: Summe aller drei Spalten, Jahr 2025, durch 1.000.000 |
| 2 | Haushalte, ein Jahr lang | 2,2 Mio. | 2025er Summe in kWh durch 3.500 kWh je Haushalt |
| 3 | Abgeregelt Januar bis September 2026 | 6,4 TWh | tage.csv: wie oben, Monate 1-9 |
| 4 | Veränderung zum selben Zeitraum 2025 | +21 % | tage.csv |
| 5 | Umspannwerke über 5 %, Wind | 77 | umspannwerke.csv: typ wind, mw >= 10, quote_2025_pct > 5 |
| 6 | Umspannwerke über 5 %, Solar | 110 | umspannwerke.csv: typ solar, mw >= 10, quote_2025_pct > 5 |
| 7 | Neue Anlagen seit Januar 2026 (Gebiete mit Daten) | 92.886 | zubau_2026.csv: Summe anlagen |
| 8 | Anteil ihrer Leistung an Umspannwerken über 5 % | 21 % | zubau_2026.csv: mw mit uw_2025_ueber_5pct = 1 geteilt durch Summe mw |

Die Zahlen zum 8. Juli 2026, 14 Uhr (rund 9 GW, 14 %) stammen aus Daten je Viertelstunde und Wetter, die hier nicht enthalten sind.

## Abdeckung und Grenzen

1. Daten veröffentlichen: Avacon, Bayernwerk, E.DIS, Schleswig-Holstein Netz, Mitnetz, Syna (redispatchdaten.de), EWE NETZ und Osthessen Netz (eigene Websites), TenneT und 50Hertz (Übertragungsnetz).
2. Keine vergleichbaren Daten: Westnetz, Netze BW, Stadtwerke wie Berlin, Hamburg, München, viele kleine Netzbetreiber.
3. Die Meldungen enthalten keine Mengen, nur Zeitraum und Drosselstufe. Die Energie ist geschätzt.
4. Die Zuordnung zu Umspannwerken ist eine Näherung (nächstgelegenes Umspannwerk).

## Quellen und Lizenzen

1. Marktstammdatenregister, Bundesnetzagentur (dl-de/by-2-0)
2. SMARD, Bundesnetzagentur | SMARD.de (CC BY 4.0)
3. netztransparenz.de (Redispatch, EEG-Bewegungsdaten)
4. redispatchdaten.de, EWE NETZ, Osthessen Netz
5. ERA5, Copernicus Climate Change Service, ECMWF
6. Verwaltungsgrenzen © BKG (dl-de/by-2-0), OpenStreetMap-Mitwirkende (ODbL)

umspannwerke.csv und zubau_2026.csv enthalten Orte und Namen aus OpenStreetMap und stehen daher unter ODbL. Bei Weiterverwendung bitte Elevated Maps und die Quellen nennen.
