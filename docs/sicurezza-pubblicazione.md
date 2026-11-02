# Sicurezza e pubblicazione

Questa pagina descrive le protezioni configurate in `.htaccess` e i controlli da completare prima di pubblicare messina-portale su un server. Per l'installazione in locale, consulta [Installazione e avvio](installazione.md).

## Protezioni di .htaccess

Il file `.htaccess` nella cartella del sito configura Apache con queste protezioni:

- **Redirect HTTPS:** le richieste in HTTP vengono reindirizzate a HTTPS.
- **Content Security Policy (CSP) stretta:** con `script-src 'self'` il browser esegue solo gli script serviti dal sito stesso. Script inline e script di altri domini vengono bloccati, quindi ogni nuovo codice JavaScript va scritto in un file sotto `assets/js/`.
- **Cartelle bloccate:** `tools/` e `config/` non sono raggiungibili dal browser.
- **Tipi di file bloccati:** Apache rifiuta le richieste di vari tipi di file che non devono essere pubblici. Per l'elenco completo, consulta `.htaccess`.

Poiché `tools/` è bloccata, gli script in quella cartella si eseguono da riga di comando con `php`, non dal browser.

## Gestione delle credenziali

Le credenziali del database e dell'area di amministrazione devono restare fuori dalla cartella pubblica del sito. Il file `messina-config.php` si trova per questo nella cartella superiore a quella del sito. Non salvare credenziali nei file del progetto, nel repository o in questa documentazione.

## Checklist prima della pubblicazione

Completa questi controlli prima di mettere online il sito:

- [ ] Crea sul server un utente MySQL riservato al sito con lo script `tools/utente_mysql.sql`, invece di usare l'utente amministratore di MySQL.
- [ ] Esegui `tools/crea_config.php` direttamente sul server, così `messina-config.php` viene creato nella cartella superiore a quella del sito, fuori dalla cartella pubblica.
- [ ] Installa un certificato valido per il dominio. Il redirect di `.htaccess` porta tutte le richieste su HTTPS, quindi senza un certificato valido i browser mostrano un errore di sicurezza.
- [ ] Pianifica backup regolari del database con `mysqldump`, perché i contenuti inseriti da `admin.php` esistono solo nel database.
