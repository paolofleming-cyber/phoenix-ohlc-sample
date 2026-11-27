# Phoenix: storico M1 per analisi e backtest

Storico canonico disponibile di **EURUSD, GBPUSD ed EURGBP**, pubblicato come copia
statica separata da Phoenix operativo. Fotografia del 27 novembre 2026, fino alle
**08:32 Europe/Rome escluse** (07:32 UTC). Ultima candela inclusa: apertura 08:31 Roma.
La giornata e il mese finali sono parziali. Non e' un feed live e non si aggiorna
automaticamente durante un test.

| Cross | Prima candela Roma | Ultima candela Roma | Candele M1 | File mensili |
|---|---|---|---:|---:|
| EURUSD | 01/01/2023 23:04 | 27/11/2026 08:31 | 1,452,418 | 47 |
| GBPUSD | 02/01/2023 00:04 | 27/11/2026 08:31 | 1,451,764 | 47 |
| EURGBP | 02/01/2023 00:15 | 27/11/2026 08:31 | 1,450,260 | 47 |

Totale: **4,354,442 candele**, 141 file mensili. Dati e copertura
compressi: circa 60.0 MB. Nessun Git LFS.

## Dove leggere

Questa pubblicazione e' nel ramo `history-m1-20261127`.
Usare [questo collegamento diretto](https://github.com/paolofleming-cyber/phoenix-ohlc-sample/tree/history-m1-20261127)
e clonare esplicitamente quel ramo; `main` conserva il campione precedente.

- [manifest.json](manifest.json): elenco completo di mesi, intervalli, righe,
  dimensioni e SHA-256 dei byte compressi e decompressi.
- `history/CROSS/ANNO/AAAA-MM.csv.gz`: un CSV compresso per cross e mese Roma.
- `coverage/CROSS.json.gz`: copertura di ogni giorno Roma e intervalli di calendario
  senza candele, in UTC reale, estremi incluso/escluso.
- [COVERAGE.md](COVERAGE.md): significato della copertura e limiti dei dati.
- [GUIDA_CLAUDE.txt](GUIDA_CLAUDE.txt): procedura di acquisizione e validazione.

Per selezionare un periodo, leggere il manifest e recuperare i soli mesi del cross
desiderato. Il clone completo e' possibile; un clone parziale permette di ridurre
il trasferimento quando l'ambiente supporta i filtri Git.

## Contratto dei dati

CSV UTF-8, separatore virgola, intestazione, fine riga LF:
`time_utc,time_rome,open,high,low,close,tick_volume`.

- `time_utc`: UTC reale, apertura della candela, formato ISO con Z.
- `time_rome`: medesimo istante con offset storico Europe/Rome.
- Non applicare correzioni aggiuntive dell'ora server MT4 e non usare un offset
  Roma fisso. I giorni del cambio d'ora possono avere 23 o 25 ore.
- Prezzi: stringhe decimali canoniche conservate, senza arrotondamenti aggiuntivi.
  Usare Decimal; un pip vale 0.0001 per tutti i tre cross.
- `tick_volume`: conteggio tick di origine, non volume monetario scambiato.
- Nessun riempimento dei buchi, intersezione delle serie, filtro delle giornate
  sfavorevoli o ricostruzione di EURGBP da EURUSD/GBPUSD.

L'esportazione e' stata letta in una singola transazione in sola lettura con
snapshot coerente. Conteggi per mese confrontati con la fonte, ordine, duplicati,
OHLC, conversione SQL/Python, copertura giornaliera, hash e compressione verificati.
Questi controlli attestano la copia e la coerenza strutturale; non dimostrano che
la fonte abbia ogni minuto di mercato o che una strategia sia eseguibile.

## Uso da un'AI senza scaricamenti manuali sul telefono

L'AI puo' clonare il repository nel proprio ambiente, fissare un commit, leggere
`git show COMMIT:manifest.json` e i file mensili come byte, verificarli e decomprimerli
con gzip in Python. Non occorre leggere milioni di righe con il lettore di pagine web.
Non servono credenziali Phoenix, accesso al database, API GitHub o permessi di scrittura.
Riportare sempre commit, cross, intervallo e copertura effettivamente utilizzati.

## Campione precedente conservato

I JSON del test dell'8 gennaio 2024 rimangono in `data/`, immutati byte per byte.
Il suo manifest riguarda soltanto quella giornata e quei due cross. Per l'intero
storico usare invece **il manifest alla radice**.
Il riferimento originale resta il commit `79f3efe5bc1b792884a4028becb2d3c4f1f06dee`.
Il nome del repository e' conservato per mantenere il collegamento gia collaudato.

Il repository contiene solo dati OHLC e documentazione, senza sorgenti operativi,
credenziali, dati di conti o collegamenti alla VPS. Nessun programma va eseguito
dal repository per leggere i dati.
