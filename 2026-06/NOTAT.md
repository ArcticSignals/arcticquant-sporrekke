# Proveniens — månedslogg 2026-06

Denne første loggen er en **retroaktiv registrering** av porteføljen etablert
2026-06-03 (logget privat, jf. METODIKK §3). Avvikene fra normal prosedyre
dokumenteres her én gang for alle; fra og med 2026-07 genereres loggene
direkte ved skanntidspunktet.

## Kildedokumentasjon

- **Valg og priser:** `output/portfolios/portefolje_v2_2026-06-03.csv`
  (byte-identisk med v1 — det fantes aldri to varianter, jf. METODIKK §3).
  Prisene er registrerte kurser på kjøpsdagen 2026-06-03.
- **Rangering:** rekkefølgen i porteføljeuttrekket (alle fem hadde
  modellscore 95/100 — uavgjort på score).
- **Regime:** `BULL`. Det finnes ikke noe lagret fullskann fra akkurat
  2026-06-03; nærmeste dokumenterte fullskann er 2026-06-05 11:30
  (`output/historikk/market_scan_2026-06-05_11-30.csv`), som viser
  Market Regime = BULL, bekreftet igjen 2026-06-12. Regimet er altså
  dokumentert ±2 dager fra valgdatoen, ikke på selve dagen.
- **Kodeversjon:** `5253fc54defb@ab23733f` er stempelet ved
  **registreringstidspunktet** (2026-06-12). Versjonsstempling
  (`utils/run_stamp.py`) eksisterte ikke 2026-06-03, så stempelet for
  koden som faktisk genererte valgene er ukjent. Parameterne var
  identiske (frosset per `PARAMETERS_FROZEN.md`); endringene mellom
  03.06 og 12.06 er validert mot baseline (se valideringslogger
  `output/engine_run_2026-06-12*.log`).

## Terminologi

Kildefilene fra 2026-06-03 bruker utgått signal-terminologi («STERK KJØP»).
Den er ikke videreført hit; loggen bruker kun modellscore og rangering
(METODIKK §7).
