# Phoenix: campione storico OHLC per analisi esterne

Questo repository contiene soltanto una copia statica del campione gia pubblico:
EURUSD e GBPUSD, candele M1 dell'8 gennaio 2024, giornata Europe/Rome.

Non contiene sorgenti Phoenix, credenziali, conti, strategie, dati personali,
configurazioni della VPS o connessioni al database operativo. Non contiene codice
da eseguire, GitHub Actions, submodule o file Git LFS.

## Dati

- `data/manifest.json`: contratto, orari, conteggi dichiarati, minuti assenti e hash.
- `data/EURUSD-2024-01-08.json`: 1.434 candele, 325.844 byte.
- `data/GBPUSD-2024-01-08.json`: 1.438 candele, 326.661 byte.

I tre JSON sono copie byte per byte del campione pubblicato il 24 novembre 2026:
https://phoenix-ohlc-test.paolofleming.chatgpt.site/

## Contratto degli orari

`time_utc` e' UTC reale; `time_rome` include l'offset storico Europe/Rome.
Il timestamp indica l'apertura della candela. La giornata inclusa va dalle
2024-01-08T00:00:00+01:00 (incluso) alle 2024-01-09T00:00:00+01:00 (escluso):
in UTC, 2024-01-07T23:00:00Z fino a 2024-01-08T23:00:00Z escluso.

I prezzi sono stringhe decimali originali: usare Decimal per i calcoli.
Per entrambi i cross, un pip = 0.0001. Tick volume e' il conteggio tick di origine.
Non sono disponibili bid/ask separati, spread, commissioni o sequenza intrabar.

## Integrita' e uso

Il manifest e' una dichiarazione da verificare, non il risultato della verifica
del lettore. Calcolare gli SHA-256 sui byte originali ottenuti da Git, prima di
decodificare JSON o modificare fine riga. `.gitattributes` disabilita la conversione
dei JSON; `git show <commit>:data/<file>` consente di leggerli direttamente.

Leggere ciascun cross separatamente. Non fare un'intersezione tra i timestamp
dei due cross e non colmare i minuti assenti. La causa delle assenze non e'
deducibile dal solo campione. Il calendario civile ha 1.440 minuti, ma questo
non implica che ogni minuto debba contenere attivita' di mercato.

Il box di confronto e' 00:15 incluso - 00:45 escluso Europe/Rome, 8 gennaio 2024:
2024-01-07T23:15:00Z incluso - 2024-01-07T23:45:00Z escluso. L'ultima candela
inclusa apre alle 00:44 Roma / 23:44 UTC. Il box e la copertura dell'intera
giornata sono due verifiche distinte. I risultati attesi del box non sono
inclusi qui, per consentire un confronto indipendente dei calcoli.

Il campione verifica acquisizione e calcolo riproducibile, non redditivita'
o eseguibilita' di una strategia, ne' completezza dell'intero storico Phoenix.
I risultati riproducibili devono riportare il commit effettivamente utilizzato.
