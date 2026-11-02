# Guida ai contenuti

Questa pagina spiega come aggiungere voci e sezioni al portale e come preparare le immagini. È rivolta a chi cura i contenuti e non richiede di modificare il codice.

## Aggiungi o modifica una voce

Usa l'area di amministrazione per inserire una nuova voce in una sezione esistente o per correggerne una.

1. Nel browser, apri `admin.php` (in locale: `http://localhost/Messina-Portale/admin.php`) e accedi.
2. Compila la voce, scegliendo la sezione a cui appartiene.
3. Salva la voce.

Il salvataggio usa l'`id` della voce: se esiste già una voce con lo stesso `id`, viene sovrascritta. Per creare una voce nuova, usa un `id` che non sia già in uso.

La sessione di amministrazione scade dopo 30 minuti e l'accesso viene bloccato dopo 5 tentativi falliti.

Le voci salvate da `admin.php` restano solo nel database. Per conservarle nel progetto, rigenera `tools/database.sql` come descritto in [Installazione e avvio](installazione.md#rigenera-toolsdatabasesql).

## Crea una nuova sezione

Ogni scheda del sito corrisponde a una riga della tabella `categorie`. Per aggiungere una sezione non devi modificare il codice: il frontend legge l'elenco delle schede da `api.php`.

- Inserisci una nuova riga nella tabella `categorie` del database `messina_portale`, per esempio con phpMyAdmin di XAMPP. Nella colonna `ordine`, indica la posizione della scheda rispetto alle altre.

  La nuova scheda compare nel sito al successivo caricamento della pagina. Dopo averla creata, aggiungi le voci da `admin.php`.

La `chiave` della sezione deve rispettare queste regole:

- contiene solo lettere minuscole, cifre, trattino basso e trattino (`[a-z0-9_-]`);
- ha al massimo 40 caratteri;
- è unica, perché le voci si collegano alla sezione proprio tramite la `chiave`.

Per esempio, `feste_patronali` è una chiave valida, mentre `Feste Patronali` non lo è.

## Prepara le immagini

Le immagini delle voci devono rispettare queste regole:

- formato WebP;
- larghezza di circa 800 px;
- peso inferiore a 150 KB;
- file già presente nella cartella `assets/img/` del sito.

Una voce può usare solo immagini presenti in `assets/img/`: copia il file nella cartella prima di indicarlo nella voce e non usare immagini ospitate su altri siti.
