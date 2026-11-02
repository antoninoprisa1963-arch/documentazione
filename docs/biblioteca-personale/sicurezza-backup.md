# Sicurezza e backup

Questa pagina descrive i limiti di sicurezza di biblioteca-personale e come salvare una copia dei tuoi dati.

## Usa l'applicazione solo in locale

> **Attenzione:** non esporre l'applicazione su Internet così com'è. Chiunque raggiunga il sito può aggiungere, modificare ed eliminare libri e autori.

L'applicazione è pensata come biblioteca personale per uso locale. Per questo le azioni di scrittura di `api.php` non hanno:

- **autenticazione:** non ci sono utenti né password;
- **protezione CSRF:** un'altra pagina aperta nel browser potrebbe inviare richieste all'applicazione a tua insaputa.

Usala solo sul tuo computer, con Apache di XAMPP raggiungibile in locale. Prima di renderla accessibile da altri computer o da Internet, aggiungi al codice l'autenticazione e la protezione CSRF.

## Protezioni presenti

- **`.htaccess`:** `Options -Indexes` impedisce di vedere l'elenco dei file delle cartelle, e l'accesso diretto ai file `.sqlite` è bloccato, quindi il database non si può scaricare dal browser.
- **Query:** tutte le query al database usano istruzioni preparate, che proteggono dalla SQL injection.
- **Immagini:** vengono accettati solo file JPG, PNG o WebP, verificati dal tipo reale del file, fino a 3 MB, e salvati con un nome casuale.
- **Errori:** in caso di errore interno l'API risponde con un messaggio generico; il dettaglio resta nel log.

## Fai un backup

Tutti i dati della biblioteca stanno in due posti: il database e le immagini caricate. Per un backup completo copia entrambi:

- il file `data/biblioteca.sqlite`;
- la cartella `uploads/`.

Fai il backup prima di operazioni che non si possono annullare, come l'eliminazione di un autore o un'importazione grande. Per avere una copia coerente, fai il backup quando non stai usando l'applicazione.

Per ripristinare, rimetti il file e la cartella al loro posto nella cartella del progetto.
