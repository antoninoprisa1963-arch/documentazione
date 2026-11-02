# Architettura di messina-portale

Questa pagina descrive come sono collegati il database, le API PHP, l'area di amministrazione e il frontend JavaScript. È pensata per chi modifica il codice del sito. Per aggiungere contenuti senza toccare il codice, consulta [Guida ai contenuti](guida-contenuti.md).

## Come funziona il sito

Il browser carica `index.html` e `assets/js/app.js`. Lo script legge l'elenco delle sezioni da `api.php` e, quando il visitatore apre una scheda, chiede le voci di quella sezione. `api.php` legge i dati dal database MySQL attraverso `db.php`.

Le scritture seguono un percorso separato: chi cura i contenuti accede ad `admin.php`, che invia i dati a `salva.php`. L'accesso è gestito da `auth.php`.

| File | Ruolo |
| --- | --- |
| `index.html` | Unica pagina pubblica del sito |
| `assets/js/app.js` | Frontend: schede, caricamento delle sezioni, cronologia, Hero animato |
| `api.php` | API pubblica in sola lettura (GET) |
| `admin.php` | Area di amministrazione per inserire e modificare voci |
| `salva.php` | Salvataggio delle voci inviate da `admin.php` |
| `auth.php` | Sessione di accesso all'area di amministrazione e token CSRF |
| `db.php` | Connessione al database |
| `sitemap.php`, `robots.php` | Generazione della sitemap e del file robots per i motori di ricerca |

## Database

Il database si chiama `messina_portale` e ha due tabelle.

- **`categorie`**: una riga per ogni scheda del sito. La colonna `chiave` identifica la sezione e la colonna `ordine` stabilisce la posizione della scheda.
- **`contenuti`**: le voci delle sezioni. Ogni voce ha un `id` unico ed è collegata alla propria sezione con una chiave esterna su `categorie.chiave`.

Poiché le schede vengono lette da `categorie`, per aggiungere una sezione basta inserire una riga nella tabella.

## API pubblica (api.php)

`api.php` accetta solo richieste GET e non modifica mai il database.

| Richiesta | Risposta |
| --- | --- |
| `api.php` | Elenco delle sezioni |
| `api.php?categoria=<chiave>` | Voci della sezione con quella `chiave` |
| `api.php?q=<testo>` | Voci che corrispondono al testo cercato |

Nelle voci restituite, il campo `descrizione_estesa` arriva già diviso in paragrafi, così il frontend non deve elaborare il testo.

## Scrittura dei contenuti (admin.php e salva.php)

`admin.php` invia le voci a `salva.php`, che esegue un upsert in base all'`id`: se esiste già una voce con lo stesso `id` la aggiorna, altrimenti ne crea una nuova.

## Autenticazione (auth.php)

`auth.php` gestisce l'accesso all'area di amministrazione con una sessione PHP:

- il cookie di sessione è `HttpOnly` e `SameSite=Strict`, quindi non è leggibile da JavaScript e non viene inviato da richieste che partono da altri siti;
- la sessione scade dopo 30 minuti;
- l'accesso viene bloccato dopo 5 tentativi falliti;
- le richieste di scrittura sono protette da un token CSRF. Il token impedisce a un altro sito di inviare dati a nome di chi ha effettuato l'accesso.

## Connessione al database (db.php)

`db.php` apre la connessione a MySQL con i dati letti da `messina-config.php`, il file di configurazione che si trova fuori dalla cartella pubblica del sito. Per crearlo, consulta [Installazione e avvio](installazione.md#crea-la-configurazione).

## Frontend (assets/js/app.js)

`app.js` richiede ogni sezione ad `api.php` solo la prima volta che il visitatore la apre. Le aperture successive usano i dati già caricati.

Quando il visitatore cambia scheda, `app.js` aggiorna l'indirizzo con `history.pushState`. In questo modo i pulsanti Indietro e Avanti del browser passano da una sezione all'altra.

L'immagine di apertura (Hero) è disegnata su un elemento `<canvas>` a partire da 48 fotogrammi WebP. Il fotogramma mostrato dipende dalla posizione di scorrimento della pagina.
