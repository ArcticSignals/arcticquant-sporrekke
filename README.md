# ArcticQuant Sporrekke

> **Ansvarsfraskrivelse:** ArcticQuant er et analyse- og forskningsverktøy,
> ikke investeringsrådgivning. Dette repoet dokumenterer en modells valg —
> ikke anbefalinger til noen person. Historisk avkastning er ingen garanti
> for fremtidig avkastning. Simulerte resultater (backtest) er ikke faktiske
> handelsresultater. Se [`METODIKK.md`](METODIKK.md) §8–9 for
> interessekonflikt og fullstendig ansvarsfraskrivelse.

Offentlig, **append-only** logg over hvilke aksjer ArcticQuant-modellen
rangerer høyest på Oslo Børs — og hvordan det faktisk går med dem.

Reglene ble erklært **før** resultatene forelå og kan ikke endres med
tilbakevirkende kraft: les [`METODIKK.md`](METODIKK.md) først.
Modellens parametere ble frosset 12.06.2026, før første offentlige logg —
alt som logges her er ekte out-of-sample.

Selve modellkoden ligger i et separat, privat repo. Hver månedslogg er
stemplet med kodeversjon, slik at det kan etterprøves hvilken kode som
genererte hvilke valg.

## Struktur

```
.
├── README.md          ← denne filen
├── METODIKK.md        ← reglene, erklært før resultatene
└── YYYY-MM/           ← én mappe per månedslogg
    ├── picks.csv      ← modellens valg, skrives én gang
    └── NOTAT.md       ← proveniens og avvik (kun ved behov)
```

## picks.csv — kolonner

| Kolonne | Innhold |
|---|---|
| `Dato` | Datoen valgene gjelder fra (YYYY-MM-DD) |
| `Rangering` | Modellens rangering (1 = høyest) |
| `Ticker` | Yahoo Finance-ticker (Oslo Børs: `.OL`) |
| `Selskap` | Selskapsnavn |
| `Sektor` | Sektor |
| `Modellscore` | Modellscore 0–100 (nøytral terminologi, jf. METODIKK §7) |
| `Regime` | Markedsregime ved valget (BULL/NEUTRAL/BEAR) |
| `Pris_NOK` | Registrert kurs på valgdatoen (entry-konvensjon i METODIKK §4) |
| `Kodeversjon` | Versjonsstempel `kildehash@git-commit` fra det private koderepoet |

## Regler (kortversjon, METODIKK §6)

1. Hver månedsfil skrives **én gang** og endres aldri i ettertid.
2. Feil korrigeres med en ny fil + forklaring; originalen blir stående.
3. Tapere fjernes aldri.
4. Git-historikken er det uavhengige tidsstempelet.
