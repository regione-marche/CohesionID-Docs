# **FAQ – Integrazione tecnica**

Informazioni tecniche rivolte a sviluppatori, sistemisti e fornitori che integrano applicazioni e servizi con Cohesion ID.

---

## Configurazione e integrazione

### **Ho bisogno di assistenza per l'integrazione di Cohesion nel mio applicativo, chi posso contattare?**
Se hai bisogno di aiuto in merito all'integrazione di Cohesion, contattaci all'indirizzo [Integrazione Cohesion](mailto:integrazionecohesion@regione.marche.it)

---

## Troubleshooting

### **L’autenticazione SPID/CIE da un’app mobile non si completa correttamente. Cosa verificare?**

Nelle applicazioni mobile il processo di autenticazione tramite **SPID o CIE** può prevedere il passaggio tra più componenti, ad esempio l’applicazione chiamante, il browser del dispositivo e, ove previsto, l’app dell’Identity Provider.

Per il corretto completamento dell’autenticazione è quindi necessario che l’applicazione gestisca correttamente l’intero flusso, mantenendone la continuità anche durante i passaggi tra contesti di navigazione differenti.

In caso di anomalie si raccomanda di verificare in particolare:

- che eventuali **WebView o componenti di navigazione integrati nell’applicazione** siano compatibili con i redirect previsti dal flusso di autenticazione e con l’eventuale apertura di applicazioni esterne;
- che il passaggio tra applicazione, browser e app dell’Identity Provider non comporti la perdita dello **stato della sessione** necessario al completamento dell’autenticazione;
- che sia correttamente configurato il **meccanismo di ritorno verso l’applicazione** al termine del processo di autenticazione;
- che gli URL di callback e i relativi meccanismi di apertura dell’app siano gestiti correttamente dal sistema operativo;
- che il flusso venga verificato sui diversi sistemi operativi, browser e modalità di autenticazione supportati dall’applicazione.

Particolare attenzione deve essere posta nei casi in cui il processo di autenticazione venga **avviato in un contesto di navigazione e prosegua o si concluda in un contesto differente**, ad esempio quando il login viene avviato all’interno dell’applicazione e, a seguito dell’interazione con l’Identity Provider, il controllo viene restituito tramite il browser del dispositivo.

In tali scenari l’integrazione deve garantire che il cambio di contesto non interrompa il flusso e che, una volta completata l’autenticazione, l’utente possa essere ricondotto correttamente all’applicazione chiamante.

In presenza di problemi è inoltre utile verificare se lo stesso flusso di autenticazione, eseguito interamente tramite browser, si conclude correttamente: questo consente di distinguere eventuali problematiche legate all’integrazione mobile da anomalie del processo di autenticazione.

