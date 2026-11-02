# Panoramica di messina-portale

messina-portale raccoglie in un unico sito la storia, i luoghi, i personaggi, la cucina e le leggende di Messina. Questa pagina spiega lo scopo del portale, i suoi utenti e il modo in cui sono organizzati i contenuti. Per installare il sito, consulta [Installazione e avvio](installazione.md).

## Scopo

Il portale presenta Messina sia a chi la visita sia a chi ci vive. Tutti i contenuti si trovano in una sola pagina, `index.html`, divisa in sezioni a schede:

- Storia
- Monumenti
- Palazzi
- Chiese
- Fontane
- Personaggi
- Cucina
- Miti e Leggende

Le sezioni non sono fisse nel codice. Ogni scheda corrisponde a una riga del database, quindi chi cura i contenuti può aggiungerne di nuove senza modificare il codice. Per i dettagli, consulta [Guida ai contenuti](guida-contenuti.md).

## Utenti

| Utente | Cosa cerca nel portale |
| --- | --- |
| Visitatori e turisti | Che cosa vedere e assaggiare a Messina, con la storia dei luoghi |
| Messinesi | Un riferimento sulla storia, sui monumenti e sulle tradizioni della propria città |
| Chi cura i contenuti | Uno strumento per aggiungere e aggiornare voci e sezioni dall'area di amministrazione (`admin.php`) |

## Posizionamento editoriale

messina-portale è un portale di divulgazione culturale e turistica. I contenuti sono organizzati per tema, con una sezione per ciascun argomento. Ogni voce presenta un luogo, un personaggio, un piatto o una leggenda con un testo esteso diviso in paragrafi.

## Tecnologie

Il sito usa PHP, JavaScript vanilla e MySQL e gira su XAMPP (Apache e MySQL). Il progetto non usa strumenti di build, gestori di pacchetti né test automatici: i file del repository sono gli stessi che Apache serve. Per la struttura del codice, consulta [Architettura](architettura.md).
