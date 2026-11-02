# Database e architettura

Questa pagina descrive come sono collegate le parti dell'applicazione e come è fatto il database SQLite. Serve a chi vuole modificare il codice o interrogare i dati direttamente.

## Come funziona

- `index.php` mostra l'interfaccia. Il codice in `assets/app.js` legge e salva i dati chiamando `api.php`.
- `api.php` riceve le richieste, valida i dati e risponde in JSON. Per l'elenco delle azioni, consulta il [Riferimento API](api.md).
- `db.php` apre la connessione SQLite, crea lo schema e applica le migrazioni.
- `fonti.php` interroga Wikidata, Wikipedia in italiano e Open Library per l'importazione di un autore dal web.

Tutte le query usano istruzioni preparate (prepared statement).

## Database

Il database è il file `data/biblioteca.sqlite`. La connessione attiva `PRAGMA foreign_keys = ON`, quindi SQLite applica i vincoli di chiave esterna.

### Tabella autori

| Colonna | Note |
| --- | --- |
| `id` | Identificativo dell'autore. |
| `nome` | `TEXT NOT NULL UNIQUE`: due autori non possono avere lo stesso nome. |
| `foto` | Foto dell'autore, salvata in `uploads/autori/`. |

### Tabella libri

| Colonna | Note |
| --- | --- |
| `id` | Identificativo del libro. |
| `titolo` | Obbligatorio (`NOT NULL`). |
| `autore_id` | Obbligatorio (`NOT NULL`). Chiave esterna su `autori` con `ON DELETE CASCADE`. |
| `titolo_originale` | Titolo nella lingua originale. |
| `coautore` | Eventuale coautore. |
| `traduttore` | Traduttore dell'edizione. |
| `protagonista` | Personaggio principale. |
| `anno` | Anno di pubblicazione. |
| `pagine` | Numero di pagine. |
| `editore` | Casa editrice. |
| `serie` | Serie a cui appartiene il libro. |
| `sinossi` | Riassunto del libro. |
| `copertina` | Copertina, salvata in `uploads/copertine/`. |
| `estratto` | Brano tratto dal libro. |

L'indice `idx_libri_autore` su `autore_id` velocizza la lettura dei libri di un autore.

### Eliminazione di un autore

Quando elimini un autore, il vincolo `ON DELETE CASCADE` elimina anche tutti i suoi libri. L'applicazione rimuove inoltre dalla cartella `uploads/` i file delle loro copertine e la foto dell'autore.

## Migrazioni

Lo schema si aggiorna con migrazioni leggere in `db.php`. Se nel database mancano le colonne `libri.titolo_originale`, `libri.estratto` o `autori.foto`, `db()` le aggiunge con `ALTER TABLE`. Un database creato con una versione precedente si aggiorna quindi da solo, senza perdere i dati.

## File caricati

| Cartella | Contenuto |
| --- | --- |
| `uploads/copertine/` | Copertine dei libri. |
| `uploads/autori/` | Foto degli autori. |
| `uploads/import/` | Copertine da usare nell'importazione da CSV. |

I file caricati ricevono un nome casuale. Per i formati accettati, consulta [Guida all'uso](guida-uso.md#gestisci-le-immagini).
