# Installazione e avvio

Questa pagina spiega come avviare biblioteca-personale sul tuo computer con XAMPP. L'applicazione non richiede configurazione né build: il database si crea da solo al primo accesso.

## Prerequisiti

- XAMPP con PHP 8 o successivo. Serve solo Apache: MySQL non viene usato.
- La cartella del progetto copiata nella cartella `htdocs` di XAMPP.

## Avvia l'applicazione

Apri l'applicazione sempre attraverso Apache. Se apri `index.php` direttamente come file dal disco, il browser non esegue PHP e l'applicazione non funziona.

1. Nel pannello di controllo di XAMPP, avvia **Apache**.
2. Nel browser, apri `http://localhost/biblioteca-personale/`. L'indirizzo corrisponde al nome della cartella del progetto dentro `htdocs`.

## Cosa succede al primo accesso

Al primo accesso, la funzione `db()` in `db.php`:

1. crea il file `data/biblioteca.sqlite`;
2. crea le tabelle `autori` e `libri`;
3. inserisce dati di esempio: quattro autori (Umberto Eco, Italo Calvino, Elena Ferrante e J.R.R. Tolkien) con otto libri.

I dati di esempio servono solo a provare l'applicazione. Eliminali e sostituiscili con i libri della tua biblioteca, a mano o con l'[importazione da CSV](guida-uso.md#importa-libri-da-un-file-csv).

## Permessi di scrittura

PHP crea il file `data/biblioteca.sqlite` al primo accesso e salva le immagini caricate in `uploads/`, nelle sottocartelle `uploads/copertine/` e `uploads/autori/`, che crea se mancano. Per questo l'utente con cui gira Apache deve poter scrivere nelle cartelle `data/` e `uploads/`.

Con XAMPP su Windows, di norma Apache ha già questi permessi e non devi configurare nulla.

## Aggiornamenti dello schema

Quando aggiorni il codice, non devi modificare il database a mano. `db()` controlla lo schema e aggiunge con `ALTER TABLE` le colonne introdotte nelle versioni successive se mancano: `titolo_originale` ed `estratto` nella tabella `libri` e `foto` nella tabella `autori`. Per i dettagli, consulta [Database e architettura](architettura.md#migrazioni).

## Prossimi passi

- Per conoscere le funzioni dell'interfaccia, consulta la [Guida all'uso](guida-uso.md).
- Prima di inserire la tua biblioteca, leggi [Sicurezza e backup](sicurezza-backup.md).
