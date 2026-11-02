# Riferimento API

`api.php` è l'API JSON che l'interfaccia usa per leggere e salvare i dati. Questa pagina elenca le azioni disponibili, per chi vuole modificare l'interfaccia o capire cosa succede dietro un'operazione.

Le azioni di scrittura non richiedono autenticazione. Leggi [Sicurezza e backup](sicurezza-backup.md) prima di rendere l'applicazione raggiungibile da altri computer.

## Formato delle richieste e delle risposte

- Con l'installazione predefinita, l'API si trova all'indirizzo `http://localhost/biblioteca-personale/api.php`.
- Ogni richiesta indica l'azione nel parametro `a` della query string, per esempio `api.php?a=autori`.
- Le letture usano `GET`. Le scritture usano `POST`: l'azione resta nella query string e i dati vanno nel corpo della richiesta.
- Le risposte sono in JSON.
- Un errore restituisce un oggetto con la chiave `errore`, per esempio `{"errore": "..."}`, con il codice HTTP corrispondente.
- Un errore interno restituisce il codice 500 con un messaggio generico. Il dettaglio dell'errore finisce solo nel log di PHP, non nella risposta.

## Letture (GET)

| Azione | Parametri | Risultato |
| --- | --- | --- |
| `autori` | Nessuno | Elenco degli autori, ciascuno con il numero dei suoi libri. |
| `autore` | `id` | L'autore e i suoi libri, ordinati per anno e poi per titolo. |
| `libro` | `id` | La scheda completa del libro, con i riferimenti al libro precedente e al successivo dello stesso autore. |
| `cerca` | `q` | Fino a 20 risultati in cui il titolo, il nome dell'autore o la serie contiene il testo cercato (ricerca `LIKE`). |
| `modello-csv` | Nessuno | Scarica il modello CSV per l'importazione: UTF-8 con BOM, separatore `;`. |
| `trova-autore` | `nome` (almeno 2 caratteri) | Persone trovate su Wikidata con quel nome. Primo passaggio dell'importazione dal web. |
| `anteprima-autore` | `qid` (identificativo Wikidata, per esempio `Q...`), `nome` | Le opere dell'autore trovate sul web, con l'indicazione di quelle già in biblioteca. |

Esempio:

```text
GET api.php?a=cerca&q=rosa
```

## Scritture (POST)

| Azione | Effetto |
| --- | --- |
| `nuovo-libro` | Aggiunge un libro. Risponde con il codice 201. |
| `modifica-libro` | Modifica un libro esistente. |
| `elimina-libro` | Elimina un libro. |
| `modifica-autore` | Modifica il nome o la foto di un autore. Risponde con il codice 409 se esiste già un altro autore con lo stesso nome. |
| `elimina-autore` | Elimina un autore, i suoi libri e i file delle loro immagini. |
| `importa-csv` | Importa libri da un file CSV. Vedi [Importa libri da un file CSV](guida-uso.md#importa-libri-da-un-file-csv). |
| `importa-autore` | Salva i libri scelti dopo `anteprima-autore`. Vedi [Importa un autore dal web](guida-uso.md#importa-un-autore-dal-web). |

### Campi delle scritture

| Azione | Campi nel corpo della richiesta |
| --- | --- |
| `nuovo-libro` | I campi del libro, descritti nella tabella seguente. |
| `modifica-libro` | `id` del libro, i campi del libro e, facoltativo, `rimuovi_copertina=1` per eliminare la copertina. |
| `elimina-libro` | `id` del libro. |
| `modifica-autore` | `id`, `nome` (obbligatorio, massimo 120 caratteri), `foto` (file immagine) e, facoltativo, `rimuovi_foto=1` per eliminare la foto. |
| `elimina-autore` | `id` dell'autore. |
| `importa-csv` | Il file CSV nel campo `file`. |
| `importa-autore` | `nome` dell'autore e `libri`: un array JSON di oggetti con `titolo`, `coautore`, `protagonista`, `anno`, `pagine`, `editore`, `serie` e `sinossi`. Al massimo 300 elementi. |

`nuovo-libro` e `modifica-libro` ricevono un form multipart con questi campi:

| Campo | Tipo e limiti |
| --- | --- |
| `titolo` | Obbligatorio, massimo 200 caratteri. |
| `autore` | Obbligatorio, massimo 120 caratteri. |
| `titolo_originale`, `coautore`, `traduttore`, `protagonista` | Massimo 200 caratteri ciascuno. |
| `anno`, `pagine` | Numeri interi. |
| `editore`, `serie` | Massimo 120 caratteri ciascuno. |
| `sinossi` | Massimo 5000 caratteri. |
| `estratto` | Massimo 20000 caratteri. |
| `copertina` | File immagine. |

## Regole di validazione dei libri

Quando aggiungi o modifichi un libro:

- Titolo e autore sono obbligatori.
- Se l'autore non esiste, viene creato. Il confronto con gli autori esistenti non distingue maiuscole e minuscole, quindi `umberto eco` corrisponde a `Umberto Eco`.
- L'anno, se indicato, deve essere compreso tra 1 e l'anno corrente più uno.
- I campi rispettano le lunghezze massime indicate in [Campi delle scritture](#campi-delle-scritture).

Per i limiti sulle immagini, consulta [Gestisci le immagini](guida-uso.md#gestisci-le-immagini).
