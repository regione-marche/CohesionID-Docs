# **Integrazione mediante servlet Java**

Con il presente si vuole fornire una panoramica completa delle modalità di integrazione e delle configurazioni di utilizzo del Service Provider fornito con Cohesion in applicativi web che richiedono autenticazione e profilazione utente.
Verrà in dettaglio descritta la procedura di installazione in ambiente Java Servlet e la configurazione per l’utilizzo classico ed il funzionamento in modalità federata SAML2.0.

> **[Scarica la Servlet Java ed il manuale](https://cohesion.regione.marche.it/cohesioninformativo/materiale/integrazione%20mediante%20Servlet.zip)**

> **[Scarica la versione per java8 e successive](https://cohesion.regione.marche.it/cohesioninformativo/Portals/0/Download/CohesionServlet-3.0.zip)**

> **[Scarica il keystore valido a partire dal 16/02/2022](https://cohesion.regione.marche.it/cohesioninformativo/Portals/0/Keystore_java.zip)**


***


## **Prerequisiti**:

* Apache Tomcat v6+, JBoss, Glassfish
* Java v 1.6+

La servlet per l’integrazione di Cohesion è stata riscritta in modo da essere compatibile anche con qualunque servlet container e non utilizzare librerie esterne e componenti aggiuntivi che ne potrebbero limitare la compatibilità.

Inoltre è stato aggiunto il supporto alla funzionalità di logout, prima assente. Abbiamo quindi la servlet “Authentication” che fornisce Single Sign On e la servlet “Logout” che effettua il logout da Cohesion. Usando questo pacchetto, Cohesion potrà essere integrato in ogni tipologia di sito a prescindere dal linguaggio di progettazione (volendo anche in un sito .NET su IIS).

Una volta configurata la servlet e lanciato l’ambiente Tomcat infatti, sarà sufficiente creare nel proprio sito una pagina di login che rediriga alla pagina servlet “Authentication” specificando il parametro ReturnUrl, che dovrà contenere l’url della pagina a cui la servlet ritornerà il token utente.

Tale pagina può essere la stessa pagina di login, l’importante è che sia in grado di processare il token utente ricevuto via POST. Il token è firmato, eventualmente crittografato, e codificato in base64, perciò la pagina di login dovrà riuscire a decodificare la stringa base64, eventualmente decriptare il token mediante la chiave impostata nella servlet (l’algoritmo utilizzato è il 3-DES/ECB/NoPadding), ed infine volendo, verificare la firma del token.

Quest’ultimo passaggio in realtà non è strettamente necessario ai fini della sicurezza qualora la connessione ai server Cohesion sia fatta in HTTPS. Questo perché la servlet controlla automaticamente che il certificato SSL della pagina corrisponda effettivamente al nome DNS usato, e in caso negativo ritorna una notifica. 

Solo dopo questi passaggi si potrà autenticare l’utente ed aggiungere il token nella propria sessione.

Nel pacchetto vengono fornite due pagine login di esempio; una sviluppata in JSP ed un'altra in PHP, insieme al certificato Cohesion necessario a verificare la firma digitale del token.

Per effettuare il logout è invece sufficiente creare una pagina di logout che rediriga alla servlet “Logout”. È possibile anche in questo caso specificare il parametro ReturnUrl che sarà l’indirizzo a cui la servlet ridirigerà l’utente una volta effettuata la disconnessione da Cohesion. Nel pacchetto vengono fornite oltre alle pagine di login anche due semplici pagine di logout, una in JSP ed una in PHP.

*Di seguito verranno elencate le operazioni di configurazione da effettuare*:

1. Effettuare il deploy del WAR: Nel caso di Apache Tomcat come servlet container, copiare il file CohesionServlet.war dentro la cartella webapp e lanciare Tomcat in modo da effettuare automaticamente il deploy della servlet. Nel caso Tomcat sia configurato per non effettuare il deploy automatico è sufficiente scompattare il file .war (è un archivio zip) nella cartella webapp/CohesionServlet.

2. Importare i certificati CA della Regione Marche presenti nel keystore RegioneMarcheCA.keystore, nel keystore interno di java mediante il comando:

```java
keytool -importkeystore -srckeystore RegioneMarcheCA.keystore
```

La password del keystore RegioneMarcheCA.keystore è “password” mentre la password dell’ keystore java di default dovrebbe essere “changeit”.

Alternativamente a questa procedura è possibile lanciare Tomcat o JBoss specificando il keystore da utilizzare per i certificati CA; 
I parametri da aggiungere alla variabile di sistema JAVA_OPTS comunemente usata in Tomcat e JBoss sono:

```java
-Djavax.net.ssl.trustStore=/path/RegioneMarcheCA.keystore
-Djavax.net.ssl.trustStorePassword=password
```
3. Copiare il certificato personale presente nel pacchetto in una cartella sicura (non accessibile via web). Nel caso il certificato abbia estensione .pfx, è fondamentale rinominarlo in .p12. Qualora non sia stato fornito un certificato personale insieme alla servlet è possibile utilizzare la modalità di recupero sicuro del token di autenticazione specificando il parametro **“webSso”** descritto nel punto 4c. In tal caso il parametro **“wsSso”** presente nel punto 4c e tutti i parametri del punto 4a potranno essere ignorati. Questa è la modalità di default con cui viene fornita la servlet.

4. Modificare il file di configurazione nella cartella webapp/CohesionServlet/WEB-INF/web.xml nel caso di Tomcat, impostando i seguenti parametri:

**a) Parametri relativi al certificato**
Qualora fosse stato rilasciato un certificato personale insieme alla servlet, questi parametri andranno obbligatoriamente cambiati in quanto sono specifici per il singolo certificato utente e sono presenti sia per la servlet di autenticazione che per quella di logout:

* **keystorePath**: Inserire il percorso del certificato personale del punto 3).
* **keystoreType**: Se il file è un .p12 o un .pfx rinominato in .p12 lasciare il valore PKCS12.
* **pwdKeystore**: La password che protegge il certificato
* **pwdCertificate**: La password che protegge la chiave privata. Solitamente è uguale a pwdKeystore
* **aliasCertificate**: L'alias del certificato. Per visualizzarlo utilizzare il comando:

```java
keytool -list -keystore <certificato>.p12 -storetype pkcs12
```
Il comando è presente nella cartella bin di Java e restituirà un output simile al seguente:

`be7446f14507a331c1b5b8ff70a66520_d0e8d7b1-bcb5-4128-9d8e-2fc4dc332132`, 30-ott-2012, PrivateKeyEntry, Impronta digitale certificato (SHA1): DC:DD:60:1C:9D:43:AF:9B:F7:6E:AE:6B:5A:D5:F7:14:33:D0:0C:5C

Il primo valore terminato dalla virgola è l'alias da inserire.

**b) Parametri relativi al token utente restituito**

Questi parametri sono opzionali e relativi solo all'invio del token tra la servlet e la pagina di login:

* **encryptionKey**:Questo parametro è opzionale. Se impostato deve avere una lunghezza fissa di 24 caratteri e permetterà di restituire alla pagina di login il token utente crittografato usando l’algoritmo 3-DES/ECB/NoPadding. Il token sarà perciò prima firmato, poi crittografato ed infine convertito in base64 e inviato in POST alla pagina di login. L’utilizzo della cifratura del token è necessario qualora si utilizzi una pagina di login non protetta da connessione sicura https. Qualora la connessione sia in https è sufficiente la firma del token per garantire un livello sicurezza elevato.
* **urlRichiesta**: Questo valore va impostato solo nel caso si scelga di non inviare il parametro ReturnUrl durante la chiamata alla servlet di autenticazione per mezzo della pagina di login. In tal caso la servlet restituirà il token utente in **POST** alla pagina specificata in **urlRichiesta**, altrimenti sarà restituito alla pagina specificata in **ReturnUrl**.
* **urlIndex**: pagina iniziale a cui reindirizzare in caso di errore. Se non viene specificata viene mostrato l'errore nella pagina in cui si presenta.

**c) Parametri relativi all'IdP Cohesion**
Per abilitare il funzionamento in modalità SAML2.0 è sufficiente cambiare i valori di **urlCheck**, **wsSso** e **additionalData** nel seguente modo:

* **urlCheck**: https://cohesion2.regione.marche.it/SPManager/WAYF.aspx
* **wsSso**: https://cohesion2.regione.marche.it/SPManager/wsCheckSessionSPM.asmx
* **additionalData**: http://yourhost/YOURSITE/Logout.aspx

Deve contenere obbligatoriamente il percorso completo della propria pagina di logout affinché il logout SAML funzioni correttamente. Può contenere altri due parametri opzionali: i livelli di autenticazione da mostrare (parametro AuthRestriction) e un flag per mostrare solo i meccanismi di accesso federato SPID, CIEID, CNS e l'accesso tramite dominio. I vari parametri vanno separati da punto e virgola ";".

Per approfondimenti in merito alla configurazione dei parametri addizionali, è possibile far riferimento alla pagina [Parametri della Richiesta di Autenticazione](/CohesionID-Docs/Parametri-della-Richiesta-di-Autenticazione) 


> Opzionale

Dopo l'URL di logout è possibile inserire la stringa eidas=1. L'inserimento di questa stringa abilita la login eIDAS nel sistema di autenticazione SAML 2.0 di Cohesion. Questa opzione è compatibile con tutte le altre fin qui indicate. 

Es.
`AuthRestriction=2,1;http://host/YOURSITE/Logout.aspx;UType=c;eidas=1`

Affinchè in modalità SAML2.0 funzioni il single logout è poi necessario che la propria pagina di logout richiami la servlet di logout passando il parametro ReturnUrl valorizzato come segue:

`…./Logout?
ReturnUrl=https%3A%2F%2Fcohesion2.regione.marche.it%2FSPManager%2FLogout.aspx`

E' possibile forzare l'entityID utilizzato dall'utente, impostando il seguente parametro nel query string    **entityID=<nome idp>:idp**

Esempio (Sviluppo):
```
https://cohesion2sviluppo.regione.marche.it/SPManager/WAYF.aspx?entityID=cohesion.regione.marche.it:idp
```
Esempio (Produzione):
```
https://cohesion2.regione.marche.it/SPManager/WAYF.aspx?entityID=cohesion2.regione.marche.it:idp
```
### **Parametro id_sito**

In caso per l'integrazione sia necessario impostare un IDSito col valore fornito da Regione Marche in fase di abilitazione all'uso di cohesion, il valore va sostituito nella classe contenuta nel WAR Java
```java
WEB-INF\classes\it\unicam\cs\cohesion\servlet\AuthServlet.java
```
riga 34 [private String idsito = "TEST";]

e nella classe
```java
WEB-INF\classes\it\unicam\cs\cohesion\servlet\LogoutServlet.java
```

riga 22 [private String idsito = "TEST";]

### **Rinnovo asserzione SAML**

Una volta autenticati, la sessione per il Single Sign On sarà valida per 60 minuti. Nella modalità SAML questo tempo sarà anche specificato nella risposta SAML ritornata dall’IdP. Entro tale tempo sarà possibile accedere a qualunque sito protetto da Cohesion senza che appaia la maschera di autenticazione. Il Service Provider in tal caso otterrà un nuovo token (contenente anche una nuova asserzione SAML in caso di modalità SAML) ed un rinnovamento del cookie di sessione per altri 60 minuti.

Qualora si volesse rinnovare la sessione dal proprio SP sarà perciò necessario cancellare il cookie di sessione del proprio sito entro i 60 minuti (es. a 50 minuti) e redirigere l’utente ad una propria pagina privata che richiede l’autenticazione per l’accesso. A questo punto il SP rigirerà l’utente all’IdP Cohesion che, vedendo ancora valida la sessione, aggiornerà il cookie di sessione di altri 60 minuti e ritornerà un nuovo token o una nuova asserzione SAML valida altri 60 minuti senza mostrare l’interfaccia di autenticazione.

