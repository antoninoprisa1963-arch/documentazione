# Riferimento API

`api.php` è l'API JSON che l'interfaccia usa per leggere e salvare i dati. Questa pagina elenca le azioni disponibili, per chi vuole modificare l'interfaccia o capire cosa succede dietro un'operazione.

Le azioni di scrittura non richiedono autenticazione. Leggi [Sicurezza e backup](sicurezza-backup.md) prima di rendere l'applicazione raggiungibile da altri computer.

## Formato delle richieste e delle risposte

- Ogni richiesta indica l'azione nel parametro `a`, per esempio `api.php?a=autori`.
- Le letture usano `GET`, le scritture usano `POST`.
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
| `modifica-autore` | Modifica un autore: campi `nome`, `foto` e `rimuovi_foto`. Risponde con il codice 409 se esiste già un altro autore con lo stesso nome. |
| `elimina-autore` | Elimina un autore, i suoi libri e i file delle loro immagini. |
| `importa-csv` | Importa libri da un file CSV. Vedi [Importa libri da un file CSV](guida-uso.md#importa-libri-da-un-file-csv). |
| `importa-autore` | Salva i libri scelti dopo `anteprima-autore`. Vedi [Importa un autore dal web](guida-uso.md#importa-un-autore-dal-web). |

Per i nomi esatti dei campi inviati con ogni azione, consulta `api.php` e il codice che li invia in `assets/app.js`.

## Regole di validazione dei libri

Quando aggiungi o modifichi un libro:

- Titolo e autore sono obbligatori.
- Se l'autore non esiste, viene creato. Il confronto con gli autori esistenti non distingue maiuscole e minuscole, quindi `umberto eco` corrisponde a `Umberto Eco`.
- L'anno, se indicato, deve essere compreso tra 1 e l'anno corrente più uno.
- I campi hanno queste lunghezze massime:

  | Campo | Lunghezza massima |
  | --- | --- |
  | Titolo | 200 caratteri |
  | Autore | 120 caratteri |
  | Sinossi | 5000 caratteri |
  | Estratto | 20000 caratteri |

Per i limiti sulle immagini, consulta [Gestisci le immagini](guida-uso.md#gestisci-le-immagini).
