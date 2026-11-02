# Documentazione

Questo repository raccoglie la documentazione di due progetti web pensati per XAMPP. Ogni progetto ha le sue pagine; scegli quello che ti interessa dall'indice.

## Indice dei progetti

| Progetto | Descrizione | Inizia da |
| --- | --- | --- |
| [messina-portale](#messina-portale) | Portale culturale e turistico dedicato a Messina, in PHP, JavaScript vanilla e MySQL. | [Panoramica](docs/panoramica.md) |
| [biblioteca-personale](#biblioteca-personale) | Gestione di una biblioteca personale (autori e libri) per uso locale, in PHP 8, JavaScript vanilla e SQLite. | [Panoramica](docs/biblioteca-personale/panoramica.md) |

## messina-portale

messina-portale è un portale culturale e turistico dedicato a Messina. Il sito ha una sola pagina (`index.html`) con sezioni a schede, come Storia, Monumenti, Chiese e Cucina. È scritto in PHP, JavaScript vanilla e MySQL e gira su XAMPP.

Questa documentazione è rivolta a chi installa il sito, ne sviluppa il codice o ne cura i contenuti.

1. [Panoramica](docs/panoramica.md): a cosa serve il portale, per chi è pensato e come è impostato dal punto di vista editoriale.
2. [Installazione e avvio](docs/installazione.md): come avviare il sito in locale con XAMPP, importare il database e creare la configurazione.
3. [Architettura](docs/architettura.md): database, API, area di amministrazione e frontend.
4. [Guida ai contenuti](docs/guida-contenuti.md): come aggiungere voci e sezioni e come preparare le immagini.
5. [Sicurezza e pubblicazione](docs/sicurezza-pubblicazione.md): le protezioni di `.htaccess` e la checklist da completare prima di andare online.

## biblioteca-personale

biblioteca-personale è un'applicazione per catalogare la propria biblioteca: autori, libri, copertine e foto. È scritta in PHP 8 e JavaScript vanilla, salva i dati in un database SQLite e gira su XAMPP con il solo Apache. È pensata per l'uso in locale e non va esposta su Internet così com'è.

Questa documentazione è rivolta a chi installa l'applicazione, la usa per gestire la propria biblioteca o ne modifica il codice.

1. [Panoramica](docs/biblioteca-personale/panoramica.md): cosa fa l'applicazione, le tecnologie, la struttura della cartella e il sottoprogetto `sito-autore/`.
2. [Installazione e avvio](docs/biblioteca-personale/installazione.md): come avviarla con XAMPP e cosa succede al primo accesso.
3. [Database e architettura](docs/biblioteca-personale/architettura.md): lo schema SQLite, le migrazioni e i file caricati.
4. [Riferimento API](docs/biblioteca-personale/api.md): le azioni di lettura e scrittura di `api.php` e le regole di validazione.
5. [Guida all'uso](docs/biblioteca-personale/guida-uso.md): aggiungere e modificare libri, importare da CSV e dal web, gestire le immagini.
6. [Sicurezza e backup](docs/biblioteca-personale/sicurezza-backup.md): i limiti di sicurezza e come salvare una copia dei dati.
