# Guida all'uso

Questa pagina spiega come inserire e tenere aggiornata la tua biblioteca. Descrive come aggiungere e modificare libri, importarli in blocco da un file CSV o dal web e gestire le immagini.

## Aggiungi e modifica libri

Per ogni libro, titolo e autore sono obbligatori; tutti gli altri campi sono facoltativi.

- **Autore:** se scrivi il nome di un autore che non è ancora in biblioteca, l'applicazione lo crea. Se l'autore esiste già, il libro gli viene associato anche se scrivi il nome con maiuscole diverse.
- **Anno:** deve essere compreso tra 1 e l'anno successivo a quello corrente.
- **Lunghezze massime:** titolo 200 caratteri, autore 120, sinossi 5000, estratto 20000.

Oltre a titolo e autore puoi indicare titolo originale, coautore, traduttore, protagonista, anno, numero di pagine, editore, serie, sinossi, estratto e copertina.

Nella scheda di un libro puoi passare al libro precedente o successivo dello stesso autore.

### Modifica o elimina un autore

Puoi cambiare il nome di un autore e aggiungere, sostituire o rimuovere la sua foto. Il nuovo nome non può coincidere con quello di un altro autore.

> **Attenzione:** se elimini un autore, vengono eliminati anche tutti i suoi libri, con i file delle copertine e la foto dell'autore. L'operazione non si può annullare: fai prima un [backup](sicurezza-backup.md#fai-un-backup).

## Importa libri da un file CSV

Usa l'importazione da CSV per inserire molti libri insieme, per esempio partendo da un foglio Excel. Il file `clive_cussler.csv` nella cartella del progetto è un esempio già pronto.

### Prepara il file

1. Scarica il modello CSV dall'applicazione (azione `modello-csv`). Il modello è in UTF-8 con BOM e usa il separatore `;`.
2. Compila una riga per libro. Le colonne del modello sono:

   `autore`, `titolo`, `titolo_originale`, `coautore`, `traduttore`, `protagonista`, `anno`, `pagine`, `editore`, `serie`, `sinossi`, `copertina`

   Solo `autore` e `titolo` sono obbligatorie. Sono accettati anche questi nomi alternativi:

   | Nome alternativo | Colonna |
   | --- | --- |
   | `co_autore` | `coautore` |
   | `anno_di_pubblicazione` | `anno` |
   | `numero_di_pagine` | `pagine` |
   | `trama` | `sinossi` |

3. Facoltativo. Per aggiungere le copertine, scrivi nella colonna `copertina` il nome del file dell'immagine e copia i file nella cartella `uploads/import/` prima dell'importazione. Durante l'importazione ogni copertina viene copiata in `uploads/` con un nome casuale.

Il file deve rispettare questi limiti:

- Dimensione massima 2 MB e al massimo 2000 righe.
- Codifica UTF-8, con o senza BOM, oppure Windows-1252, la codifica dei CSV salvati da Excel.
- Separatore `;`, `,` o tabulatore. L'applicazione lo riconosce da sola.

### Importa il file

- Carica il file CSV dall'interfaccia (azione `importa-csv`).

I libri già presenti in biblioteca, cioè con lo stesso autore e lo stesso titolo, vengono saltati. Il confronto avviene su autore e titolo normalizzati, non sul testo esatto. Al termine l'applicazione mostra:

- le righe lette;
- i libri aggiunti;
- i libri saltati, con l'elenco di quelli già presenti;
- gli errori riga per riga.

Gli elenchi dei già presenti e degli errori mostrano al massimo 30 voci ciascuno.

L'importazione dei dati è atomica sul database; eventuali copertine già copiate prima di un errore possono restare in `uploads/` e si possono eliminare a mano.

## Importa un autore dal web

Usa questa importazione per inserire le opere di un autore senza scriverle a mano. Richiede un collegamento a Internet, perché i dati vengono letti da Wikidata, da Wikipedia in italiano e da Open Library.

1. Cerca l'autore per nome (almeno 2 caratteri). L'applicazione mostra le persone corrispondenti trovate su Wikidata.
2. Scegli la persona giusta. L'applicazione mostra l'anteprima delle sue opere, con i riassunti da Wikipedia in italiano e i dati da Open Library. Le opere che hai già in biblioteca sono segnalate.
3. Seleziona le opere da aggiungere e conferma. Puoi importare al massimo 300 libri per volta; i titoli già presenti vengono saltati.

Controlla i dati importati: provengono da fonti pubbliche e possono essere incompleti o diversi dall'edizione che possiedi.

## Gestisci le immagini

Puoi caricare una copertina per ogni libro e una foto per ogni autore.

- **Formati accettati:** JPG, PNG o WebP. L'applicazione controlla il tipo reale del file, non solo l'estensione: un file rinominato in `.jpg` che non è un'immagine JPG viene rifiutato.
- **Dimensione massima:** 3 MB.
- **Nome del file:** ogni immagine viene salvata con un nome casuale, quindi il nome originale non viene conservato.

Le copertine sono salvate in `uploads/copertine/` e le foto degli autori in `uploads/autori/`.
