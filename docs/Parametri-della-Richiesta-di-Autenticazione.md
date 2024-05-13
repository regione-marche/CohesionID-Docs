# **Parametri della Richiesta di Autenticazione**
In questa sezione definiamo in dettaglio i parametri di autenticazione presenti nel tag `stilesheet` della Richiesta di Autenticazione effettuata da Cohesion.
```xml
<?xml version="1.0" encoding="utf-16"?>
<dsAuth xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://tempuri.org/Auth.xsd"
<auth>
<user />
<id_sa />
<id_sito>SITE_ID</id_sito>
<esito_auth_sa />
<id_sessione_sa />
<id_sessione_aspnet_sa />
<url_validate>
https://cohesion2.regione.marche.it/TestCohesion/ValidateFE.aspx?ReturnUrl=%2ftestcohesion%2fprivato%2fpagina_protetta.aspx</url_validate>
<url_richiesta>
https://cohesion2.regione.marche.it:443/testcohesion/privato/pagina_protetta.aspx</url_richiesta>
<esito_auth_sso />
<id_sessione_sso />
<id_sessione_aspnet_sso />
<stilesheet>AuthRestriction=3,2,1,0;
https://cohesion2.regione.marche.it/testcohesion/Index.aspx;UType=d;purpose=PG|PF|LP;aggregato=1;eidas=3;SpidMode=OIDC;CieMode=OIDC;IPA=c_000</stilesheet>
</auth>
</dsAuth>

```

## **`AuthRestriction`**
Il parametro `AuthRestriction` stabilisce il livello di autenticazione minimo richiesto associandolo a un valore numerico (0,1,2,3). Nello specifico:

0= Accesso solo con password - SPID Livello 1

1= Accesso con password e PIN o token OTP - SPID Livello 2

2= Accesso con smartcard - SPID Livello 3

3= Accesso con Dominio Active Directory REGIONEMARCHE

Ad esempio, impostando il parametro come
`AuthRestriction=3,2,1` stabiliamo che l'accesso sarà consentito ad utenze che dispongono di credenziali SPID di Livello 2 e 3, nonché tramite Dominio, ma non sarà possibile utilizzare credenziali SPID di primo livello.

## **URL di reindirizzamento**
L'URL al quale l'utente sarà reindirizzato dopo aver effettuato l'autenticazione.
## **`UType`**
Specifica la tipologia di utente che accede tramite Cohesion. Si distinguono 3 differenti tipologie:

`UType=c` = Accesso per soli cittadini, l'autenticazione tramite credenziali Cohesion è disabilitata

`UType=d` = Accesso per soli Dipendenti della PA, viene abilitata l'autenticazione tramite credenziali Cohesion

`UType=cd` = Accesso per Dipendenti delle PA e Cittadini, l'autenticazione tramite credenziali Cohesion è abilitata (opzione di default)

Ad esempio, impostando il parametro come segue `UType=c` stabiliamo che l'unica tipologia di utenza accettabile è il cittadino, disabilitando l'accesso tramite credenziali Cohesion


## **`purpose`**
Definisce il livello di sicurezza SPID in base alla tipologia (o scopo) dell'utente. Si distinguono in:

PF = SPID personale per Persona Fisica, costituisce l'identità di un cittadino che accede ai servizi della PA

LP = SPID del Libero Professionista, racchiude i dati i dati della persona fisica ed eventuali attributi che caratterizzano la sua professione.

PG = SPID Giuridico, rilasciata ad una persona fisica a cui vengono associati anche i dati della persona giuridica per cui opera.

Ad esempio, impostando il parametro come `purpose=PG` Cohesion consentirà l'accesso solamente alle persone dotate di SPID Giuridico.

Per la lista completa dei gestori di Identità Digitali che supportano le tipologie PG e LP, si prega di consultare il seguente link: https://www.agid.gov.it/it/piattaforme/spid

## **`aggregato`**
Se l'ente è federato in modalità aggregata, sarà necessario inserire il parametro `aggregato=1`

Se non specificato, verrà utilizzata la modalità di default diretta

## **`eidas`**
Abilita il login tramite il nodo italiano eIDAS specificando se, a valle dell'autenticazione, è necessaria la verifica del Codice Fiscale:

`eidas=1` = Autenticazione eidas abilitata, nessuna verifica del Codice Fiscale.

`eidas=2` = Verifica semplice del Codice Fiscale.

`eidas=3` = Verifica del Codice Fiscale con supporto fisico.

Ad esempio, impostando il parametro come `eidas=3` sarà necessaria la verifica del Codice Fiscale tramite supporto, escludendo i livelli di autenticazione più bassi.

## **`SpidMode`**
Definisce il protocollo di autenticazione che CohesionID deve utilizzare per comunicare con la piattaforma nazionale SPID. 

Se non specificato, verrà utilizzato il protocollo di default SAML. Impostando il parametro come `SpidMode=OIDC` verrà utilizzato il nuovo protocollo OpenID Connect 

## **`CieMode`**
Definisce il protocollo di autenticazione che CohesionID deve utilizzare per comunicare con la piattaforma nazionale CIE. 

Se non specificato, verrà utilizzato il protocollo di default SAML mentre, impostando il parametro come `CieMode=OIDC`, verrà utilizzato il nuovo protocollo OpenID Connect 

## **`IPA`**
L'IPA è l'Indice delle Pubbliche Amministrazioni, univoco ad ogni ente della Pubblica Amministrazione. 

Specificando questo parametro, CohesionID effettuerà la richiesta di autenticazione verso le piattaforme nazionali ed europee per conto dell'ente aggregato specificato.
