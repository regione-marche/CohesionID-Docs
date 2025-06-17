# **Ambiente di collaudo** 
A supporto di enti e fornitori nel processo di integrazione con il sistema CohesionID, è stato predisposto un ambiente di collaudo che consente di verificare il corretto funzionamento delle configurazioni implementate.

## Requisiti per l'accesso

L'accesso all'ambiente di collaudo è riservato a enti o fornitori che:

- hanno completato la procedura di integrazione con CohesionID;
- hanno ricevuto l'ID Sito univoco assegnato dal team tecnico Cohesion.

L’ambiente di collaudo sarà disponibile dal giorno solare successivo all’avvenuta federazione del sistema. Per maggiori informazioni in merito alla richiesta di integrazione e all'ottenimento dell'ID Sito, è possibile fare riferimento all'apposita sezione: [Richiesta integrazione](https://cohesion.regione.marche.it/CohesionDocs/Pubbliche-Amministrazioni-e-Imprese/#richiesta-di-integrazione)

## Configurazione dell'ambiente di collaudo

Per effettuare una richiesta di autenticazione in ambiente di collaudo, è necessario inviare una chiamata in GET all'indirizzo:

[https://cohesioncollaudo.regione.marche.it/SPManager/WAYF.aspx](https://cohesioncollaudo.regione.marche.it/SPManager/WAYF.aspx)

La richiesta deve includere il parametro `AUTH`, contenente i parametri necessari alla gestione dell'autenticazione. Per dettagli sulla struttura del parametro e sulla configurazione della richiesta, è possibile fare riferimento alla sezione: [Flusso di autenticazione](https://cohesion.regione.marche.it/CohesionDocs/Flusso-di-Autenticazione/#1-lutente-richiede-di-effettuare-il-login)

## Credenziali di test

In ambiente di collaudo **non è possibile effettuare autenticazioni tramite SPID, CIE o altri sistemi di identità digitale.**

Per finalità di verifica vengono messe a disposizione due utenze fittizie CohesionID:

**Utente 1**  
- Codice Fiscale: TSTTNT80A01H501O  
- Password/PIN: 13579113

**Utente 2**  
- Codice Fiscale: TSTTNT80A41H501S  
- Password/PIN: 24681012

> **ATTENZIONE**  
> Le credenziali di test sono valide esclusivamente nell'ambiente di collaudo e **non abilitano l'accesso ai servizi in produzione**.
