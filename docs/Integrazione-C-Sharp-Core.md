# **Integrazione C# Core**

Con il presente si vuole fornire una panoramica completa delle modalità di integrazione e delle configurazioni di utilizzo del Service Provider fornito con Cohesion in applicativi web che richiedono autenticazione e profilazione utente.
Verrà in dettaglio descritta la procedura di installazione in ambiente IIS mediante classe C# CohesionSSO.cs e la configurazione per l’utilizzo classico ed il funzionamento in modalità federata SAML2.0.
Questa modalità è sempre preferibile e in particolare è necessaria qualora non venga rilasciato all’ente un certificato personale per la comunicazione con Cohesion.

## **Come integrarlo nel proprio applicativo**

>  **[Demo asp net core](https://github.com/regione-marche/Cohesion2NETCore)**

In questa pagina è presente un'applicativo di test di autenticazione Cohesion su asp net core in modo da poter provare le funzionalità.
Nella repo della demo sono indicati i requisiti ed i passaggi da eseguire per integrare l'autenticazione cohesion nel proprio applicativo vengono indicati i requisiti e come fare per integrare Cohesion.



## **CohesionService.cs**

Questo service mette a disposizione una serie di metodi che permettono di gestire tutti i passaggi dell'autenticazione tramite cohesion partendo dal login fino alla richiesta di logout.

### **Il metodo "RequestAuth"**

> `RequestAuth(string urlValidation, string urlReturn)`

crea un base64 di una richiesta di login a cohesion dove sono richiesti 2 parametri:

* urlValidation - ovvero l'url del metodo che andrà a gestire la validazione della richiesta di login
* urlReturn - ovvero l'url dove si vorrà reindirizzare l'utente una volta effettuata la richiesta di login con successo

infine ritorna un url dove verrà rindirizzato l'utente per effettuare correttamente il login di cohesion.

### **Il metodo "CheckAuth"**

> `CheckAuth(string auth)`

verifica se la richiesta di login a cohesion ha avuto esito positivo o negativo restituendo un oggetto "AuthCohesionCheckResponse".

### **Il metodo "LogoutFE"**

> `LogoutFE(_session)`

è utilizzato per effettuare la richiesta logout passando al metodo la sessione attuale dell'utente dove è possibile, se presenti, estrapolare i parametri di login ed effettuare correttamente il logout.

## **appsettings.json**

Nel file appsettings.json è necessario impostare dei valori per il corretto funzionamento dell'autenticazione tramite cohesion.

> In particolare andranno impostate le seguenti chiavi:

*  **sso.check.url** : parametro fisso della pagina SSO di Cohesion.
*  **webCheckSessionSSO** : parametro fisso della pagina di recupero token di autenticazione.
*  **sso.additionalData** : parametri opzionali di autenticazione.
*  **site.URLLogout** : url della propria pagina di logout relativo alla posizione della pagina protetta; è possibile specificare il parametro ReturnUrl con l'url della pagina a cui ritornare una volta effettuato il logout.
*  **site.id_sito**: IDSito fornito da Regione Marche la momento della abilitazione alla integrazione cohesion.
*  **site.IndexURL**: url della propria pagina a cui ritornare in caso di errore di autenticazione.

Esempio di codice di configurazione in **appsettings.json**:
```c#
"AppSettings": {
    "SSOcheckUrl": "https://cohesion2.regione.marche.it/SPManager/WAYF.aspx",
    "SSOwebCheckSession": "https://cohesion2.regione.marche.it/SPManager/webCheckSessionSSO.aspx",
    "SSOadditionalData": "AuthRestriction=0,1,2,3;https://MYSITE/Account/LogOff",
    "SiteIndexUrl": "https://MYSITE/Account/Login",
    "SiteUrlLogout": "../Logout.aspx?ReturnUrl=https%3A%2F%2Fcohesion2.regione.marche.it%2FSPManager%2FLogout.aspx",
    "SiteIdSito": "MYSITE"
  }
```

Di seguito verranno descritti i campi che è possibile cambiare:

* **site.URLLogout**: Contiene l’url della pagina di Logout dalla posizione della pagina autenticata. A differenza della modalità classica non è però possibile cambiare il parametro ReturnUrl in quanto strettamente dipendente alle funzionalità di logout di SAML.
* **site.IndexURL**: se presente deve contenere l’url della propria pagina a cui ritornare in caso di errore di autenticazione.
* **sso.additionalData**: Deve contenere obbligatoriamente il percorso completo della propria pagina di logout affinché il logout SAML funzioni correttamente. Può contenere altri due parametri opzionali: i livelli di autenticazione da mostrare (parametro AuthRestriction) e un flag per mostrare solo i meccanismi di accesso federato SPID, CIEID, CNS e l'accesso tramite dominio. I vari parametri vanno separati da punto e virgola ";".

Per approfondimenti in merito alla configurazione dei parametri addizionali, è possibile far riferimento alla pagina [Parametri della Richiesta di Autenticazione](/CohesionID-Docs/Parametri-della-Richiesta-di-Autenticazione)


> [Opzionale]

Prima dell'URL di logout è possibile inserire la stringa AuthRestricion=0,1,2,3 dove i valori 0,1,2,3 indicano i livelli di autenticazione da mostrare nella pagina di login dell’Idp federato selezionato, in particolare:

* 0 indica di mostrare l'autenticazione SAML "Password"
* 1 indica di mostrare l'autenticazione SAML "Smartcard"
* 2 indica di mostrare l'autenticazione SAML "SmartcardPKI"
* 3 indica di mostrare l'autenticazione SAML "Kerberos"

I seguenti valori sono standard SAML e dovrebbero essere riconosciuti per ogni IdP federato selezionato. Nel caso venga scelto Cohesion come Idp allora i valori saranno interpretati con le modalità di autenticazione già descritte nel punto a), a meno che non venga specificato l'altro parametro opzionale UType=c. 

E’ possibile nascondere o visualizzare le modalità di autenticazione togliendo o aggiungendo i rispettivi valori separati da una virgola. L’ordine non è influente.

> [Opzionale]

Dopo l'URL di logout è possibile inserire la stringa eidas=1. L'inserimento di questa stringa abilita la login eIDAS nel sistema di autenticazione SAML 2.0 di Cohesion. Questa opzione è compatibile con tutte le altre fin qui indicate. 

ES: AuthRestriction=2,1;http://host/YOURSITE/Logout.aspx;UType=c;eidas=1


