# Copertura e limiti dello storico

La pubblicazione comprende tutte le righe canoniche disponibili dei tre cross
entro il limite temporale fissato nel manifest. "Tutto lo storico disponibile"
non significa una candela per ogni minuto di calendario.

## Ogni cross conserva la propria serie

EURUSD, GBPUSD ed EURGBP possono avere minuti e coperture diversi. EURGBP e'
esportato dalla propria serie canonica, non ricavato dividendo gli altri cross.
Una finestra completa su EURUSD non dimostra che lo sia anche su EURGBP.
Per il test della fascia 00:15-00:45 Roma, verificare le 30 aperture di ogni
giorno/cross: non supporre che tutti i lunedi siano assenti o tutti presenti.

## File di copertura

`coverage/CROSS.json.gz` contiene per ogni giorno:

- `calendar_minutes`: durata reale della giornata Roma, 1380/1440/1500 minuti.
- `assessed_minutes`: minuti compresi nella fotografia esportata; l'ultimo giorno
  e' parziale. I minuti futuri rispetto al limite non sono classificati assenti.
- `rows`: candele osservate nel giorno.
- `absent_calendar_minutes`: assessed_minutes meno rows.
- `absent_ranges_utc`: intervalli UTC [inizio incluso, fine esclusa] senza barre.
- `partial_day`, `first_utc`, `last_utc`: estremi e stato della giornata.

Gli intervalli comprendono anche ore a mercato chiuso e giorni prima della
prima candela disponibile nel mese iniziale. Nessuna causa e' dedotta dal solo
silenzio della serie. Per finestre di test e' possibile verificare esattamente
quali minuti mancano. Non trasformare la copertura del calendario in una misura
di affidabilita' del broker o in un calendario ufficiale di contrattazione.

## Limiti delle simulazioni

I dati sono OHLC M1 della fonte canonica: non includono tick completi, bid/ask
separati, spread, commissioni o sequenza intrabar. Non permettono da soli una
ricostruzione certa dell'ordine stop/target quando entrambi ricadono nella stessa
candela. Ogni ipotesi aggiunta nel backtest va riportata come ipotesi.

L'orario Roma e' storico e dipende dalla data. Non correggere nuovamente i dati
per l'ora server MT4. Per aggregare timeframe ancorati alla sessione del broker
occorre definire quell'ancoraggio separatamente; qui vengono fornite le M1.
