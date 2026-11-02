# Panoramica

biblioteca-personale è un'applicazione web per catalogare gli autori e i libri della propria biblioteca. Per ogni libro conserva dati come anno, editore, serie, sinossi e copertina. È pensata per l'uso in locale su XAMPP, sul computer di chi la usa, e non per essere pubblicata su Internet.

## Cosa puoi fare

- Sfogliare l'elenco degli autori, con il numero di libri di ciascuno, e aprire la scheda di ogni libro.
- Cercare per titolo, nome dell'autore o serie.
- Aggiungere, modificare ed eliminare libri e autori, con copertine e foto degli autori.
- Importare molti libri insieme da un file CSV, anche salvato da Excel.
- Importare le opere di un autore da fonti web (Wikidata, Wikipedia in italiano e Open Library), con un'anteprima prima di salvare.

## Tecnologie

- **PHP 8:** il codice usa funzionalità introdotte in PHP 8, come il tipo `never` e `str_starts_with`, quindi non funziona con versioni precedenti.
- **JavaScript vanilla:** l'interfaccia usa JavaScript e CSS senza framework (`assets/app.js` e `assets/style.css`).
- **SQLite:** i dati stanno in un unico file, `data/biblioteca.sqlite`. MySQL non serve.

Non ci sono file di configurazione né passaggi di build.

## Struttura della cartella

| Percorso | Contenuto |
| --- | --- |
| `index.php` | Interfaccia dell'applicazione. |
| `api.php` | API JSON usata dall'interfaccia. |
| `db.php` | Connessione al database e creazione dello schema. |
| `fonti.php` | Importazione delle opere di un autore da fonti web. |
| `assets/` | `app.js` e `style.css` dell'interfaccia. |
| `data/biblioteca.sqlite` | Database SQLite. |
| `uploads/` | Copertine e foto degli autori caricate. |
| `clive_cussler.csv` | File CSV di esempio. |
| `.htaccess` | Protezioni per Apache. |
| `sito-autore/` | Sottoprogetto separato, descritto più sotto. |

## Il sottoprogetto sito-autore

La cartella `sito-autore/` contiene un progetto separato: un sito statico dedicato a un autore, generato con [Vite](https://vite.dev/). Non fa parte dell'applicazione della biblioteca e non serve per usarla.

Per lavorarci servono Node.js e l'installazione delle dipendenze con `npm install` dentro `sito-autore/`. Gli script disponibili in `package.json` sono `fetch`, che esegue `node scripts/fetch-author.mjs`, `dev`, `build` e `preview`. I sorgenti sono in `src/main.js` e `src/style.css`.

## Pagine di questa documentazione

1. [Installazione e avvio](installazione.md): come avviare l'applicazione con XAMPP.
2. [Database e architettura](architettura.md): come sono organizzati i file, lo schema SQLite e le migrazioni.
3. [Riferimento API](api.md): le azioni di lettura e scrittura di `api.php`.
4. [Guida all'uso](guida-uso.md): aggiungere e modificare libri, importare da CSV e dal web, gestire le immagini.
5. [Sicurezza e backup](sicurezza-backup.md): i limiti di sicurezza dell'applicazione e come fare un backup.
