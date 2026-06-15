# ArcticQuant Live Sporrekke — Metodikk

> **STATUS: PUBLISERT (2026-06-15)** i
> [`ArcticSignals/arcticquant-sporrekke`](https://github.com/ArcticSignals/arcticquant-sporrekke)
> sammen med første månedslogg (juni 2026). Git-commit-historikken er det
> autoritative publiseringstidspunktet (jf. §6.4). Alle avgjørelser ble
> bekreftet før publisering; se endringsloggen.

Dette dokumentet erklærer reglene for ArcticQuants offentlige sporrekke **før**
resultatene foreligger. Reglene kan ikke endres med tilbakevirkende kraft;
enhver senere endring datostemples i endringsloggen nederst og gjelder kun fremover.

---

## 1. Formål

Dokumentere, offentlig og uredigerbart, hvilke aksjer modellen rangerer høyest —
og hvordan det faktisk går med dem. Sporrekka er ekte out-of-sample: parameterne
ble frosset 12.06.2026 (`PARAMETERS_FROZEN.md`), før første offentlige logg.

Backtest-resultatene (2015–2025) er **simulert historikk** med kjente forbehold
(survivorship bias, in-sample-tuning) og inngår ikke i sporrekka. Sporrekka er
det eneste som teller som dokumentert resultat.

## 2. Modell og parametere

- Alle strategiparametere er frosset per `PARAMETERS_FROZEN.md`
  (frys 12.06.2026, baseline-stempel `38ae8be77267@dd7f5c99`).
- Hver månedslogg stemples med kodeversjon (`utils/run_stamp.py`) slik at det
  alltid kan etterprøves hvilken kode som genererte hvilke valg.
- Bugfikser i motoren er tillatt, men dokumenteres med før/etter-kjøring mot
  baseline. Re-tuning på historiske data er ikke tillatt.

## 2.1 Rangering og uavgjort (tiebreak)

Modellen rangerer kandidatene etter modellscore (0–100). Når to eller flere
aksjer har **samme modellscore** ved seleksjonsgrensen, avgjøres rekkefølgen av
**normalisert relativ styrke (Normalized RS) — høyest først**. Gjenstående
uavgjort (lik score *og* lik Normalized RS) faller tilbake på en stabil,
reproduserbar rekkefølge.

Regelen er deklarert 2026-06-15 og gjelder **kun live-seleksjon fra og med
juli 2026-loggen**. Den er ikke anvendt med tilbakevirkende kraft — verken på
juni 2026-loggen (der alle fem hadde score 95; rekkefølgen er dokumentert i
`2026-06/NOTAT.md`) eller på den frosne backtest-baselinen
(`PARAMETERS_FROZEN.md`), som beholder sin opprinnelige rangering. Formålet er
en fullt spesifisert, etterprøvbar seleksjon — **ikke** en forventet
avkastningsforbedring: Normalized RS har svak prediktiv kraft på 12-måneders
sikt, men er den faktoren modellen allerede vekter høyest, og er derfor et
prinsipielt og dokumenterbart valg av tiebreak.

## 3. Kanonisk portefølje

Det finnes **én** offisiell modellportefølje.

- Etablert 2026-06-03 med de fem topp-rangerte aksjene (modellscore 95/100):
  ZAP.OL, NAPA.OL, ATEA.OL, SMOP.OL, NHY.OL. Startkapital 10 000 kr,
  tilnærmet likevektet, kurtasje 29 kr per handel.
- Merknad om proveniens: filene `portefolje_v1_2026-06-03.csv` og
  `portefolje_v2_2026-06-03.csv` er byte-identiske duplikater av samme
  porteføljeuttrekk; etiketten «v2» i den interne loggen refererer til denne
  ene porteføljen. Det er altså ikke valgt mellom to varianter på grunnlag
  av resultat — det fantes aldri to.
- Porteføljen ble logget **privat** 2026-06-03. Den offisielle, offentlige
  sporrekka regnes fra datoen for første offentlige commit av denne mappen.
  Perioden 2026-06-03 → publisering rapporteres, men merkes som logget privat.

## 4. Entry- og exit-konvensjoner

- **Historiske kjøp (2026-06-03):** registrerte kurser på kjøpsdagen, som
  dokumentert i loggen.
- **Fremtidige posisjoner:** offisiell sluttkurs (EOD) første handelsdag etter
  at månedsloggen er generert og committet. Ingen intradag-kurser, ingen skjønn.
- **Holdeperiode og rebalansering:** som i frosne parametere — 12 måneders
  holdeperiode, årlig rebalansering, porteføljestørrelse etter markedsregime
  (`REGIME_TOP_N`). Live-porteføljen rebalanseres **årlig per juni**
  (etableringsmåneden, første gang juni 2027), slik at holdeperioden blir
  fulle 12 måneder fra etablering 2026-06-03. Parameterfrysens
  1. januar-regel gjelder kun backtest-simuleringen.
- Transaksjonskostnader: faktisk kurtasje (29 kr/handel) trekkes fra.

## 5. Avkastningsmåling

- **Totalavkastning:** utbytte medregnes (utbyttejusterte kurser,
  `auto_adjust=True`), konsistent med benchmarken.
- **Benchmark:** `OBX.OL` = OBX Total Return Index (verifisert 12.06.2026),
  målt fra samme dato som posisjonene. Epler mot epler: begge sider inkluderer
  utbytte.
- **Manglende data:** rapporteres eksplisitt som `N/A` med forklaring — aldri
  stille fallback til kjøpspris eller estimat.
- Delisting, suspensjon eller ticker-bytte loggføres med dato og håndtering
  (realisert verdi ved siste omsettelige kurs).

## 6. Append-only-regler

1. Hver månedsfil skrives én gang og endres aldri i ettertid.
2. Feil korrigeres ved en **ny** fil med forklaring — originalen blir stående.
3. Tapere fjernes aldri. Alle valg modellen har gjort, forblir i loggen.
4. Git-commit-historikken er det uavhengige tidsstempelet. Endringer i
   historiske filer vil synes i historikken — det er en funksjon, ikke en risiko.

## 7. Terminologi

Sporrekka bruker utelukkende nøytrale formuleringer:

- «Modellscore 87/100», «Rangering #3», «Topp-rangert» — aldri kjøps- eller
  salgsanbefalinger.
- «Modellen rangerte X høyest» — aldri «du bør kjøpe X».
- Alle tall tidsstemples og merkes med datakilde og forsinkelse (EOD-data).

Eldre interne filer kan inneholde utgått signal-terminologi; den fases ut og
inngår ikke i publiserte logger.

## 8. Interessekonflikt

Utvikleren eier per 12.06.2026 ingen av aksjene som omtales. Fra juli 2026
har utvikleren til hensikt å investere egne midler etter samme modellstrategi,
i en **egen, privat portefølje** med egen startdato (planlagt 12.07.2026) og
egen årlig rebalansering per juli. Den private porteføljen tar utgangspunkt i
modellens rangering på eget kjøpstidspunkt og kan derfor inneholde andre
aksjer enn den kanoniske modellporteføljen (seksjon 3). Egne kjøp skjer til
markedskurs, alltid **etter** at den relevante månedsloggen er committet, og
inngår aldri i sporrekkas tall — den måles utelukkende fra de registrerte
kursene i månedsloggene. Dette opplyses i hver publiserte månedsrapport, og
vesentlige endringer i egne posisjoner loggføres fremover.

## 9. Ansvarsfraskrivelse

ArcticQuant er et analyse- og forskningsverktøy, ikke investeringsrådgivning.
Sporrekka dokumenterer en modells valg, ikke anbefalinger til noen person.
Historisk avkastning er ingen garanti for fremtidig avkastning. Simulerte
resultater (backtest) er ikke faktiske handelsresultater og markedsføres aldri
som forventet avkastning.

---

## Endringslogg

| Dato | Endring |
|---|---|
| 2026-06-12 | Første utkast (upublisert). |
| 2026-06-12 | `[BEKREFT]`-punkter avgjort: rebalansering årlig per juni (seksjon 4); interessekonflikt presisert til «kan eie enkeltaksjer, speiler ikke porteføljen» (seksjon 8). Status endret til klar for publisering. |
| 2026-06-12 | Seksjon 8 revidert (før publisering): utvikleren eier per i dag ingen aksjer, men har til hensikt å speile modellporteføljen med egne midler fra juli 2026. Erstatter formuleringen fra tidligere samme dag. |
| 2026-06-12 | Seksjon 8 presisert (før publisering): egne midler investeres i en egen privat portefølje etter samme strategi, med egen startdato (planlagt 12.07.2026) og egen årlig rebalansering per juli — ikke en speiling av den kanoniske porteføljen. Kjøp skjer alltid etter at månedsloggen er committet. |
| 2026-06-15 | Publisert som `ArcticSignals/arcticquant-sporrekke` sammen med juni 2026-loggen. (Et tidligere utkast var feilaktig stemplet «publisert 2026-06-12»; rettet til faktisk publiseringsdato før første offentlige commit.) Fra og med dette tidspunktet gjelder append-only-reglene fullt ut. |
| 2026-06-15 | Ny §2.1: deklarert tiebreak for live-seleksjon — ved lik modellscore rangeres høyest Normalized RS først. Gjelder kun fremover (første gang juli 2026-loggen); ikke anvendt på juni-loggen eller den frosne backtest-baselinen. Implementert i live-skanneren; backtest-motoren bevisst urørt. |
