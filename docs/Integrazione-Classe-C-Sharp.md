# **Integrazione classe C#**


Con il presente si vuole fornire una panoramica completa delle modalità di integrazione e delle configurazioni di utilizzo del Service Provider fornito con Cohesion in applicativi web che richiedono autenticazione e profilazione utente.
Verrà in dettaglio descritta la procedura di installazione in ambiente IIS mediante classe C# CohesionSSO.cs e la configurazione per l’utilizzo classico ed il funzionamento in modalità federata SAML2.0.
Questa modalità è sempre preferibile e in particolare è necessaria qualora non venga rilasciato all’ente un certificato personale per la comunicazione con Cohesion.


>  **[Scarica l'applicazione di test e la classe di integrazione](https://github.com/regione-marche/Cohesion2NETFramework)**

Per integrare l’autenticazione in Cohesion nel proprio sito è sufficiente aggiungere la classe C# CohesionSSO.cs al proprio progetto .NET e richiamare i metodi ValidateFE e LogoutFE rispettivamente nel codice della pagina che gestisce la form authentication e della pagina di logout. I metodi vanno richiamati dopo aver inizializzato la classe mediante i costruttori disponibili.
Di seguito un esempio di codice da inserire nella pagina di form authentication:

`CohesionSSO cohesionSSO = new CohesionSSO(Request, Response, Session);
 cohesionSSO.ValidateFE();`

Una volta autenticati il token di autenticazione verrà automaticamente salvato nella variabile di sessione “_TOKEN_”.

In **TestCohesion.zip** è presente un esempio di progetto .NET 2010 in cui si mostra come integrare Cohesion mediante la classe CohesionSSO.

**La classe dispone di 2 costruttori**:


***

**Costruttore base** con i soli parametri per recuperare le variabili di richiesta http, risposta http e sessione http.
In questo modo tutti i parametri richiesti dovranno essere specificati nel web.config. 

> In particolare andranno impostate le seguenti chiavi nel web.config:

*  **sso.check.url** : parametro fisso della pagina SSO di Cohesion.
*  **webCheckSessionSSO** : parametro fisso della pagina di recupero token di autenticazione.
*  **sso.additionalData** : parametri opzionali di autenticazione.
*  **site.URLLogout** : url della propria pagina di logout relativo alla posizione della pagina protetta; è possibile specificare il parametro ReturnUrl con l'url della pagina a cui ritornare una volta effettuato il logout.
*  **site.id_sito**: IDSito fornito da Regione Marche la momento della abilitazione alla integrazione cohesion.
*  **site.IndexURL**: url della propria pagina a cui ritornare in caso di errore di autenticazione.

Esempio di codice di configurazione in **web.config**:
```c#
<appSettings>
 <add key="sso.check.url" value="http://cohesion2.regione.marche.it/SSO/Check.aspx"/>
 <add key="sso.webCheckSessionSSO" value="https://cohesion2.regione.marche.it/SSO/webCheckSessionSSO.aspx"/>
 <add key="sso.additionalData" value=""/>
 <add key="site.URLLogout" value="../Logout.aspx?ReturnUrl=index.aspx"/>
 <add key="site.id_sito" value="MYSITE"/>
 <add key="site.IndexURL" value="index.aspx"/>
</appSettings>
```

***

**Costruttore completo** con tutti i parametri richiesti:
In questo modo non si avrà bisogno di specificare i parametri nel web.config ma si passeranno direttamente al costruttore:

`CohesionSSO cohesionSSO = new CohesionSSO(request, response, session,
SSOCheckUrl, webCheckSessionSSOUrl, additionalData, indexUrl);`


***
Di seguito viene descritto un esempio di configurazione di questi parametri per utilizzare rispettivamente la modalità di funzionamento classica di Cohesion e la modalità di funzionamento federato mediante standard SAML2.0

**Modalità di funzionamento SAML2.0**

```c#
<appSettings>
 <add key="sso.check.url" value="http://cohesion2.regione.marche.it/SPManager/WAYF.aspx"/>
 <add key="sso.webCheckSessionSSO" value="https://cohesion2.regione.marche.it/SPManager/webCheckSessionSSO.aspx"/>
 <add key="site.URLLogout" 
 value= "../Logout.aspx?ReturnUrl=https%3A%2F%2Fcohesion2.regione.marche.it%2FSPManager%2FLogout.aspx"/>
 <add key="site.IndexURL" value="index.aspx"/>
 <add key="site.id_sito" value="MYSITE"/>
 <add key="sso.additionalData" value="http://YOURHOST/YOURSITE/Logout.aspx"/>
</appSettings>
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

I seguenti valori sono standard SAML e dovrebbero essere riconosciuti per ogni IdP federato selezionato. Nel caso venga scelto Cohesion come Idp allora i valori saranno interpretati con le modalità di autenticazione già descritte nel punto a), a meno che non venga specificato l'altro parametro opzionale UType=c. E’ possibile nascondere o visualizzare le modalità di autenticazione togliendo o aggiungendo i rispettivi valori separati da una virgola. L’ordine non è influente.

> [Opzionale]

Dopo l'URL di logout è possibile inserire la stringa eidas=1. L'inserimento di questa stringa abilita la login eIDAS nel sistema di autenticazione SAML 2.0 di Cohesion. Questa opzione è compatibile con tutte le altre fin qui indicate. 
Es. AuthRestriction=2,1;http://host/YOURSITE/Logout.aspx;UType=c;eidas=1


