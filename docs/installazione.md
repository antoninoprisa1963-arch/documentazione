# Installazione e avvio in locale

Questa pagina spiega come avviare messina-portale sul tuo computer con XAMPP, come importare ed esportare il database e come creare il file di configurazione. Per pubblicare il sito su un server, consulta [Sicurezza e pubblicazione](sicurezza-pubblicazione.md).

## Prerequisiti

- XAMPP installato, con Apache e MySQL.
- La cartella del progetto copiata nella cartella `htdocs` di XAMPP con il nome `Messina-Portale`.
- Accesso a un prompt dei comandi con `mysql.exe` e `php` raggiungibili. In un'installazione standard di XAMPP su Windows si trovano in `C:\xampp\mysql\bin\` e `C:\xampp\php\`.

## Avvia il sito

Apri il sito sempre attraverso Apache. Se apri `index.html` direttamente come file dal disco, il sito non funziona: le sezioni vengono caricate da `api.php` e il browser da solo non esegue PHP.

1. Nel pannello di controllo di XAMPP, avvia **Apache** e **MySQL**.
2. Nel browser, apri `http://localhost/Messina-Portale/`.

Al primo avvio, prima che il sito mostri le sezioni, crea la configurazione e importa il database con le procedure che seguono.

## Crea la configurazione

Il sito legge i dati di connessione al database da `messina-config.php`. Questo file si trova nella cartella superiore a quella del sito, così resta fuori dalla cartella pubblica servita da Apache. In locale, quindi, si trova accanto alla cartella `Messina-Portale` dentro `htdocs`.

- Dalla cartella del sito, esegui lo script di configurazione:

  ```bat
  php tools\crea_config.php
  ```

  Lo script crea `messina-config.php` nella cartella superiore.

Le credenziali del database devono restare fuori dalla cartella pubblica del sito. Non salvarle nei file del progetto e non inserirle nel repository.

## Importa il database

Il file `tools/database.sql` contiene la struttura e i contenuti del database `messina_portale`.

> **Attenzione:** l'import sovrascrive le sezioni e le voci che hanno lo stesso `id` di quelle presenti in `tools/database.sql`. Le modifiche fatte da `admin.php` a quelle righe vanno perse. Fai un backup prima di importare.

1. Dal prompt dei comandi, fai un backup del database attuale:

   ```bat
   mysqldump.exe -u <utente_mysql> -p messina_portale > backup_messina_portale.sql
   ```

2. Importa `tools/database.sql`:

   ```bat
   mysql.exe -u <utente_mysql> -p messina_portale < tools\database.sql
   ```

Il comando di import presuppone che il database `messina_portale` esista già. Sostituisci `<utente_mysql>` con il tuo utente MySQL. I comandi chiedono la password al momento dell'esecuzione. Usa il Prompt dei comandi (`cmd.exe`): in PowerShell il reindirizzamento con `<` non funziona.

## Rigenera tools/database.sql

Quando modifichi i contenuti da `admin.php`, le modifiche restano solo nel database. Per salvarle nel progetto, rigenera `tools/database.sql`.

- Dalla cartella del sito, esegui:

  ```bat
  php tools\esporta_database.php
  ```

  Lo script riscrive `tools/database.sql` con il contenuto attuale del database.

## Controlla la sintassi PHP

Il progetto non ha test automatici. Dopo aver modificato un file PHP, controlla almeno che non contenga errori di sintassi:

```bat
php -l api.php
```

Ripeti il comando per ogni file PHP che hai modificato. Se il file è corretto, `php -l` risponde `No syntax errors detected`.
