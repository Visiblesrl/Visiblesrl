# VISIBLE Srl — sito web

Repository del progetto web di **VISIBLE Srl — Concessionaria Pubblicitaria e Soluzioni Integrate**.

Il sito di riferimento è [www.visiblesrl.eu](https://www.visiblesrl.eu); il contatto aziendale è **info@visiblesrl.eu**. Il progetto presenta impianti e servizi pubblicitari, comunicazione digitale, sviluppo di prodotti informatici, progetti realizzati e VISIBLE Card, con moduli per richieste di preventivo e informazioni.

## Stato del repository

Questa è una **base iniziale**, non una copia completa del sito operativo. Alla verifica del 31 ottobre 2026, il codice applicativo versionato comprende soltanto configurazione e header condiviso, aggiunti il 26 luglio 2026. Mancano pagina iniziale, fogli di stile, immagini, moduli, amministrazione e database. Il solo clone non permette quindi di navigare il sito completo.

**Attenzione:** `includes/config.php` contiene ancora riferimenti a un'autoconcessionaria, indirizzi sul dominio `visiblesrl.it`, un telefono segnaposto e l'opzione `SHOW_DEMO_CARS`. Questi valori non descrivono il progetto VISIBLE e devono essere corretti prima di utilizzare il codice in produzione. Anche l'indirizzo fisico presente va verificato.

## Struttura attuale

| Percorso | Contenuto |
| --- | --- |
| `README.md` | Documentazione del progetto |
| `includes/config.php` | Costanti del sito, destinatari email, colori e impostazioni dimostrative |
| `includes/header.php` | Apertura HTML e header; carica la configurazione con `__DIR__` e richiama `/assets/css/style.css` |

Il foglio `assets/css/style.css` richiamato dall'header **non è presente**. Non risultano manifest Composer o npm, script di build, migrazioni o test automatizzati.

### Componenti del sito operativo da recuperare e verificare

La struttura seguente deriva dal lavoro sul sito operativo e **non è ancora versionata qui**. Percorsi e dipendenze vanno confrontati con il backup approvato prima dell'importazione.

| Percorso/componente | Funzione |
| --- | --- |
| `public/index.php` | Home e ingresso del sito |
| `public/assets/` | CSS, JavaScript e immagini, incluso `images/VISIBLE-logo-header.png` |
| `public/admin/` | Area di amministrazione |
| `public/phpmailer/` | Invio email tramite PHPMailer |
| Database SQLite `visible.sqlite` | Dati del CMS; posizione effettiva da verificare |
| Configurazione database e posta | Nei precedenti pacchetti compaiono percorsi `config/`, `conf/` e `database/`: occorre uniformarli |
| `richiesta.php` | Richiesta preventivo |
| `visible-card.php` | Presentazione VISIBLE Card |
| `informazioni-visible-card.php` | Richiesta informazioni VISIBLE Card |
| `pubblicita-visible.php` | Approfondimento sui servizi pubblicitari collegati alla Card |

## Requisiti PHP e strumenti

- Git e un interprete PHP CLI per lo sviluppo locale; server web con esecuzione PHP per la pubblicazione.
- I due file attuali usano sintassi compatibile con PHP 7.0 o successivo; questo è un limite sintattico, non una versione consigliata per il deployment.
- Il sito operativo è stato configurato su Aruba con **PHP 8.0.30**. Tale dato descrive l'ambiente storico e non certifica la compatibilità dell'intero sito con altre versioni. Per un nuovo rilascio scegliere una versione PHP supportata dal provider e validare prima il pacchetto completo in un ambiente di prova.
- Per il CMS SQLite: estensioni `PDO` e `pdo_sqlite`, da verificare sul codice recuperato; cartella dati scrivibile dall'applicazione e protetta dall'accesso HTTP.
- Per la posta: libreria PHPMailer e connettività SMTP; verificare `openssl` se la configurazione scelta usa TLS.

Non è richiesto alcun comando Composer/npm per i file attualmente presenti. Le dipendenze del sito completo dovranno essere documentate dopo il recupero.

## Installazione e verifica locale

1. Clonare il repository:

   ```bash
   git clone https://github.com/Visiblesrl/Visiblesrl.git
   cd Visiblesrl
   ```

2. Controllare interprete, moduli e sintassi:

   ```bash
   php -v
   php -m
   php -l includes/config.php
   php -l includes/header.php
   ```

3. Esaminare `includes/config.php`. Prima dell'uso applicativo correggere descrizione, URL, email e dati aziendali; rimuovere o sostituire i riferimenti alle auto. Non inserire password SMTP o altri segreti nel repository.

4. Per visualizzare il solo header iniziale:

   ```bash
   php -S 127.0.0.1:8000 -t .
   ```

   Aprire [http://127.0.0.1:8000/includes/header.php](http://127.0.0.1:8000/includes/header.php). È un controllo parziale: il CSS manca, l'header non è una pagina completa e non esiste ancora un `index.php`. Il server integrato PHP serve soltanto allo sviluppo locale.

5. Dopo avere recuperato il sito completo, verificare la document root effettiva. Se il pacchetto viene organizzato con ingresso in `public/index.php`, avviarlo con:

   ```bash
   php -S 127.0.0.1:8000 -t public
   ```

   Questo comando è applicabile solo dopo l'importazione: la cartella `public/` non esiste nell'attuale repository. Configurare separatamente dati di prova, SQLite e SMTP, senza copiare dati personali di produzione nei commit.

## Pubblicazione su Aruba

Il sito di riferimento utilizza Aruba Hosting Basic Windows. Non è presente in questo repository una procedura automatica di deploy; aggiornare il README su GitHub non pubblica il sito su Aruba.

1. Recuperare e confrontare il pacchetto operativo approvato. Riferimento storico: `VISIBLE-SRL-SMTP25-ARUBA-18-09-2026-LOGO-NUOVO.zip`; verificare anche le successive correzioni del logo. Non sovrascrivere il sito con questa sola base iniziale.
2. Eseguire un backup di file, configurazione e database attualmente online; conservarlo fuori dall'area pubblica e annotare come ripristinarlo.
3. Provare il pacchetto completo con la versione PHP scelta e verificare la document root Aruba. Nei caricamenti precedenti è stata usata una cartella `public/`: controllare il mapping effettivo per evitare cartelle duplicate e percorsi errati.
4. Caricare i file applicativi mediante File Manager Aruba o un accesso di trasferimento già verificato. Mantenere separati segreti, database di produzione e contenuti caricati dagli utenti.
5. Verificare i percorsi degli include rispetto ai file reali. Un precedente errore HTTP 500 è stato risolto correggendo il riferimento da `../config/database.php` a `config/database.php`; non applicare la sostituzione indiscriminatamente. Preferire percorsi basati su `__DIR__`.
6. Configurare la posta sul server: Aruba aveva confermato il funzionamento di **smtp.aruba.it, porta 25, con autenticazione** il 19 settembre 2026. Verificare nuovamente i parametri consentiti dall'hosting e la modalità di cifratura prima del rilascio; conservare le credenziali fuori da Git.
7. Verificare tramite HTTPS home, menu desktop/mobile, immagini, pagine VISIBLE Card, amministrazione e moduli. Per il logo usare `VISIBLE-logo-header.png` ed eliminare i richiami obsoleti a `VISIBLE-logo-nuovo-trasparente.png`.
8. Controllare log PHP, risposte HTTP e ricezione effettiva delle richieste a `info@visiblesrl.eu`. In caso di regressioni ripristinare il backup verificato.

## Prossime attività tecniche

Le attività seguenti raccolgono il lavoro già emerso per il sito e i punti rilevati in questa base; non attestano funzionalità già implementate nel repository.

- [ ] **Allineare Git e sito operativo:** recuperare il pacchetto completo approvato, confrontarlo con la produzione e importare soltanto sorgenti e asset necessari.
- [ ] **Correggere la configurazione iniziale:** identità pubblicitaria VISIBLE, dominio `visiblesrl.eu`, email aziendali e dati di contatto verificati; rimuovere contenuti demo auto e telefono segnaposto. Il cellulare personale non va reintrodotto nei contatti pubblici.
- [ ] **Uniformare cartelle e include:** definire document root, posizione di configurazione e database; risolvere i riferimenti incoerenti tra `includes/`, `config/` e `conf/` e completare CSS/header condiviso.
- [ ] **Consolidare grafica e navigazione:** logo corretto, tagline blu scuro leggibile, menu responsive e sezione “I nostri progetti”.
- [ ] **Verificare i moduli:** richiesta preventivo con Partita IVA e Codice Fiscale; informazioni VISIBLE Card con dati aziendali e referente; validazione lato server, gestione errori e prova SMTP completa.
- [ ] **Riallineare le pagine VISIBLE Card:** pagine dedicate, collegamento “Scopri di più” a `pubblicita-visible.php` e contenuti commerciali approvati da riconfermare prima della pubblicazione.
- [ ] **Completare la gestione Card digitale:** QR code e database iscritti con gestione riservata VISIBLE, controlli di accesso e informativa sul trattamento dei dati; non presumere che siano già implementati.
- [ ] **Consolidare il CMS:** verificare SQLite e permessi; preservare le modifiche già previste al login con `password_verify` e `session_regenerate_id`.
- [ ] **Preparare il prossimo rilascio PHP:** verificare compatibilità di applicazione e PHPMailer con la versione scelta, documentare dipendenze e parametri senza segreti.
- [ ] **Rendere ripetibile la manutenzione:** aggiungere esclusioni Git per segreti, database e backup, controlli di sintassi PHP e una checklist di pubblicazione e ripristino.

## Contributi e manutenzione

Mantenere separati aggiornamenti della documentazione e modifiche applicative. Per ogni modifica al sito descrivere il problema, i file coinvolti e le verifiche eseguite; aggiornare questo README quando cambiano struttura, requisiti o procedura di pubblicazione.

Non caricare in Git password, credenziali di amministrazione, database con dati personali, log contenenti informazioni riservate o archivi di backup della produzione.
