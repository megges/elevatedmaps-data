# Abgeregelt (Curtailed): data for the story

Story (German): https://elevatedmaps.com/stories/abgeregelt/
Publisher: Elevated Maps (Marcus Lehmann). Data up to 2026-10-06.

Curtailment means a grid operator throttles wind or solar units because the grid is full. The electricity is never generated.
A German draft law would let grid operators declare a substation "capacity-limited" if more than 5 % of the possible electricity there was curtailed in a year.

## Notes for AI agents

1. Load the CSV files below (UTF-8, comma separated, dot as decimal mark). Calculate with code, not in your head. The easiest way into your code environment is `git clone --depth 1 https://github.com/megges/elevatedmaps-data` (folder `abgeregelt/`). If that fails, read the links below and transfer only the file you need for the question.
2. First check your calculation against the key figures. If you deviate, say so.
3. With every answer, name file, filter and calculation.
4. The data cover only part of Germany (see coverage). Missing values mean "no data", not "no curtailment".
5. Curtailed amounts in the distribution grid are estimates. Say so when you cite them.
6. Do not answer from memory what the data do not cover. Say that it is missing.
7. Answer in the user's language.

## Files

| # | File | Rows | Size |
|---|---|---|---|
| 1 | https://elevatedmaps.com/data/abgeregelt/csv/days.csv?v=f04e8ba3 | 644 | 20 KB |
| 2 | https://elevatedmaps.com/data/abgeregelt/csv/substations.csv?v=9c502632 | 1124 | 83 KB |
| 3 | https://elevatedmaps.com/data/abgeregelt/csv/districts.csv?v=56f923cc | 400 | 22 KB |
| 4 | https://elevatedmaps.com/data/abgeregelt/csv/new_units_2026.csv?v=e20563df | 675 | 37 KB |

All files with this description: https://github.com/megges/elevatedmaps-data (folder `abgeregelt/`, files directly in it, no `csv/` subfolder)

1. **days.csv**: lost electricity per day since 2025 in MWh, no gaps (0 = no curtailment reported that day). Distribution grid estimated (throttled capacity times local weather, ERA5), transmission grid as reported (netztransparenz.de). Plus wind and solar electricity fed in across all of Germany (SMARD), for a rough share of lost electricity. Since only some grid operators publish data, that share is a lower bound.
2. **substations.csv**: one row per substation (110 kV, OpenStreetMap) and energy type, year 2025, distribution grid only. Each unit counts towards its nearest substation. Only substations with at least 3 units, at least 1 MW and at least 50 % of capacity covered. `share_curtailed_2025_pct` = curtailed / (curtailed + fed in) * 100. Amounts calibrated per unit and month against actual 2025 feed-in (EEG settlement data). `units` and `mw` = all EEG units in operation in the Marktstammdatenregister snapshot used for this story. That snapshot already contains part of the units commissioned in 2026, so `mw` is not the 2025 stock. Do not use it as the base for growth rates.
3. **districts.csv**: all German districts (Landkreise and kreisfreie Städte). `coverage` = share of wind and solar capacity at grid operators that publish data (0 to 1, empty = unknown). Values per energy type only for the 5 distribution grid operators with unit keys, at least 3 units, otherwise empty.
4. **new_units_2026.csv**: units commissioned from 1 January 2026 (Marktstammdatenregister) per substation and energy type, only at substations with data. `substation_over_5pct_2025` = 1 if the substation was over 5 % for this energy type in 2025 (at least 10 MW). Some rows have no match in substations.csv because that file only lists substations with at least 3 units and 1 MW; all of them have the flag 0.

## Key figures of the story

| # | Figure | Value | Calculation |
|---|---|---|---|
| 1 | Curtailed 2025, distribution plus transmission grid | 7.8 TWh | days.csv: sum of the three curtailment columns, year 2025, divided by 1,000,000 |
| 2 | Households supplied for a year | 2.2 million | 2025 total in kWh divided by 3,500 kWh per household |
| 3 | Curtailed January to September 2026 | 6.4 TWh | days.csv: as above, months 1-9 |
| 4 | Change vs. same period 2025 | +21 % | days.csv |
| 5 | Substations over 5 %, wind | 77 | substations.csv: type wind, mw >= 10, share_curtailed_2025_pct > 5 |
| 6 | Substations over 5 %, solar | 110 | substations.csv: type solar, mw >= 10, share_curtailed_2025_pct > 5 |
| 7 | New units since January 2026 (areas with data) | 92,886 | new_units_2026.csv: sum of units |
| 8 | Share of their capacity at substations over 5 % | 21 % | new_units_2026.csv: mw where substation_over_5pct_2025 = 1 divided by total mw |

The figures for 8 July 2026, 2 pm (about 9 GW, 14 %) come from 15-minute data and weather not included here.

## Coverage and limits

1. Publish data: Avacon, Bayernwerk, E.DIS, Schleswig-Holstein Netz, Mitnetz, Syna (redispatchdaten.de), EWE NETZ and Osthessen Netz (own websites), TenneT and 50Hertz (transmission grid).
2. No comparable data: Westnetz, Netze BW, municipal utilities such as Berlin, Hamburg, Munich, many small grid operators.
3. Reports contain no amounts, only period and throttling level. Energy is estimated.
4. Assignment to substations is an approximation (nearest substation).

## Sources and licenses

1. Marktstammdatenregister, Bundesnetzagentur (dl-de/by-2-0)
2. SMARD, Bundesnetzagentur | SMARD.de (CC BY 4.0)
3. netztransparenz.de (redispatch, EEG settlement data)
4. redispatchdaten.de, EWE NETZ, Osthessen Netz
5. Contains modified Copernicus Climate Change Service information (ERA5, ECMWF)
6. Administrative boundaries © BKG (dl-de/by-2-0), OpenStreetMap contributors (ODbL)

substations.csv and new_units_2026.csv contain locations and names from OpenStreetMap and are therefore licensed under ODbL. When reusing, please credit Elevated Maps and the sources.
