# **Flusso di Autenticazione** 
## **1. L'utente richiede di effettuare il login** 

L'utente desidera effettuare l'accesso ad un'applicazione che integra il sistema Cohesion, cliccando sul _login_ viene reindirizzato al sistema Cohesion ID.

## **2. L'utente sceglie l'IdP**

L'utente seleziona l'_Identity Provider_ per effettuare la procedura di riconoscimento. 
Cohesion genera una richiesta di accesso in cui ottiene il **Codice Fiscale** e l'**ID Sessione** dell'utente.

## **3. Accesso all'applicativo**

Cohesion effettua una seconda chiamata al fine ottenere gli attributi dell'utente recuperati dall'Identity Provider scelto dall'utente.

L'utente viene reindirizzato all'applicazione con un _token proprietario_, contente gli attributi necessari all'accesso.

## **Auth Request**

Per accedere ai servizi offerti da CohesionID, è necessario effettuare una chiamata in GET all'indirizzo [https://cohesion2.regione.marche.it/SPManager/WAYF.aspx](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fcohesion2.regione.marche.it%2FSPManager%2FWAYF.aspx) con un parametro _AUTH_ contenente dei parametri di autenticazione come quelli dell'esempio in basso.

Il chiamante, in questo esempio, sarà *example.cohesion.it*:

```xml
<?xml version="1.0" ?>
<dsAuth xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://tempuri.org/Auth.xsd">
	<auth>
		<user/>
		<id_sa/>
		<id_sito>ID SITO ASSEGNATO</id_sito>
		<esito_auth_sa/>
		<id_sessione_sa/>
		<id_sessione_aspnet_sa/>
		<url_validate>https://example.cohesion.it/cohesionAuthCallback</url_validate>
		<url_richiesta>https://example.cohesion.it:443/cohesionErrorCallback</url_richiesta>
		<esito_auth_sso/>
		<id_sessione_sso/>
		<id_sessione_aspnet_sso/>
		<stilesheet>AuthRestriction=1,2,3</stilesheet>
	</auth>
</dsAuth>
```
In particolare:

* **id_sito**: è l'identificativo assegnato all'integrazione specifica con Cohesion. Ogni sito ha un id_sito differente e univoco
* **url_validate**: contiente l'URL al quale Cohesion reindirizza i dati ottenuti dall'Identity Provider utilizzato (Poste, Namirial, Cohesion, ecc...) dopo la login
* **url_richieste**: contiene l'URL della pagina a cui tornare in caso di errori di autenticazione
* **stilesheet**: contiente (opzionalmente) l'URL al quale Cohesion fa il **redirect** dopo aver eseguito la cancellazione della sessione in seguito ad una richiesta di logout. Contiene anche eventuali restrizioni sul livello di autenticazione richiesto (SPID livello 1, 2 o 3). Per specificare il livello di autenticazione è sufficiente inserire il parametro `<AuthRestriction=1,2,3>` indicando il numero del livello minimo accettabile (per accettare ogni tipo di autenticazione, impostare il parametro con 0,1,2,3)

Esempio:
```xml
<?xml version="1.0" ?>
<dsAuth xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://tempuri.org/Auth.xsd">
	<auth>
		<user/>
		<id_sa/>
		<id_sito>COHESION_EXAMPLE</id_sito>
		<esito_auth_sa/>
		<id_sessione_sa/>
		<id_sessione_aspnet_sa/>
		<url_validate>https://example.cohesion.it/Login</url_validate>
		<url_richiesta>https://example.cohesion.it</url_richiesta>
		<esito_auth_sso/>
		<id_sessione_sso/>
		<id_sessione_aspnet_sso/>
		<stilesheet>AuthRestriction=2,3;https://example.cohesion.it/LogOff</stilesheet>
	</auth>
</dsAuth>
```

In questo esempio, diciamo a Cohesion che:

* vogliamo che il token di autenticazione venga inoltrato in POST (per default) all'indirizzo https://example.cohesion.it/Login
* vogliamo che, una volta fatto il logout, Cohesion reindirizzi all'URL https://example.cohesion.it/LogOff
* il livello di autenticazione minimo accettabile è il 2 (Codice fiscale + password + PIN se l'IdP è Cohesion oppure SPID livello 2 negli altri casi).
* se ci sono errori, vogliamo che Cohesion torni alla homepage https://example.cohesion.it

L'XML contenuto nel parametro auth deve essere codificato in **base64** e poi con **URL Encode**. Un esempio di prodotto finale ha questa forma:

```xml
PD94bWwgdmVyc2lvbj0iMS4wIiA%2FPg0KPGRzQXV0aCB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIi
B4bWxucz0iaHR0cDovL3RlbXB1cmkub3JnL0F1dGgueHNkIj4NCgk8YXV0aD4NCgkJPHVzZXIvPg0KCQk8aWRfc2EvPg0KCQk8aWRfc2l0bz5leGFtcGxl
PC9pZF9zaXRvPg0KCQk8ZXNpdG9fYXV0aF9zYS8+DQoJCTxpZF9zZXNzaW9uZV9zYS8+DQoJCTxpZF9zZXNzaW9uZV9hc3BuZXRfc2EvPg0KCQk8dXJsX3Z
hbGlkYXRlPmh0dHBzOi8vZXhhbXBsZS5jb2hlc2lvbi5pdC9Mb2dpbjwvdXJsX3ZhbGlkYXRlPg0KCQk8dXJsX3JpY2hpZXN0YT5odHRwczovL2V4YW1wb
GUuY29oZXNpb24uaXQ8L3VybF9yaWNoaWVzdGE+DQoJCTxlc2l0b19hdXRoX3Nzby8+DQoJCTxpZF9zZXNzaW9uZV9zc28vPg0KCQk8aWRfc2Vzc2lvbm
VfYXNwbmV0X3Nzby8+DQoJCTxzdGlsZXNoZWV0PkF1dGhSZXN0cmljdGlvbj0yLDM7aHR0cHM6Ly9leGFtcGxlLmNvaGVzaW9uLml0L0xvZ09mZjwvc3Rpb
GVzaGVldD4NCgk8L2F1dGg+DQo8L2RzQXV0aD4

```

## **Cosa contiene il token Cohesion dopo l'autenticazione?**

Dopo l'autenticazione dell'utente, Cohesion effettua una chiamata **POST** all'indirizzo specificato in **url_validate** nel cui body verrà specificato un token di autenticazione. 
Nel body ci sarà un elemento *auth* codificato in base64 e URL Encoded che conterrà un XML di questo tipo:

```xml

<?xml version="1.0" ?>
<dsAuth xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://tempuri.org/Auth.xsd">
	<auth>
		<user>RSSMRA85D18F051Y</user>
		<id_sa/>
		<id_sito>example</id_sito>
		<esito_auth_sa/>
		<id_sessione_sa/>
		<id_sessione_aspnet_sa/>
		<url_validate>https://example.cohesion.it/Login</url_validate>
		<url_richiesta>https://example.cohesion.it</url_richiesta>
		<esito_auth_sso>OK</esito_auth_sso>
		<id_sessione_sso>A5D64899EBCEE1FA6B3E8FB3D3D358D7E5843EA25EDFD62277CACE8A5EF23B1C5E6FAB916F333B0AFB0FAE6C4CDF7B8E8DEE461E8661DB04F5FFB01BC0EF9FCB90EEA2ACA7988119EFE17D5C3BB58BC890D75D724D434D7D60C2F451F6A51D1DFFA900EF0AAA631A51458860C6CEC7B1EBC525561624208066DF276314F41F6EC258CA3F</id_sessione_sso>
		<id_sessione_aspnet_sso>ufrq00x4f0cq2yog2ozv4lxx;https://idp.namirialtsp.com/idp;https://example.cohesion.it/LogOff;AAdzZWNyZXQxGLSQhZFb4TSPKKPNCGfO5MaCcJ99DiuEnIzpO4YvtOrgf+6qvwyEVOguFDCVbyPcZ2hhjbBtPtuCWUGs+FYX9zREfvZ1heshK7yzE24MU7JsIBK17atMdbL31QbDPNKbH2wiswXhL2CJGjY=;AAdzZWNyZXQxGLSQhZFb4TSPKKPNCGfO5MaCcJ99DiuEnIzpO4YvtOrgf+6qvwyEVOguFDCVbyPcZ2hhjbBtPtuCWUGs+FYX9zREfvZ1heshK7yzE24MU7JsIBK17atMdbL31QbDPNKbH2wiswXhL2CJGjY=;</id_sessione_aspnet_sso>
		<stilesheet>AuthRestriction=1,2,3;https://example.cohesion.it/LogOff</stilesheet>
		<ip xmlns="">cohesion2.regione.marche.it</ip>
		<AuthRestriction xmlns="">1,2,3</AuthRestriction>
		<UType xmlns="">CD</UType>
		<purpose xmlns=""/>
		<url_logout xmlns="">https://example.cohesion.it/LogOff</url_logout>
		<cittad xmlns="">1</cittad>
	</auth>
</dsAuth>
```
Queste informazioni contengono solamente il codice fiscale dell'utente, ma non i suoi attributi.
Per ottenere gli attributi completi, è necessario effettuare una chiamata GET o POST all'indirizzo [https://cohesion2.regione.marche.it/SPManager/webCheckSessionSSO.aspx](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fcohesion2.regione.marche.it%2FSPManager%2FwebCheckSessionSSO.aspx) con i seguenti parametri:

```html
Operation=GetCredential&IdSessioneSSO={id_sessione_sso}&IdSessioneASPNET={id_sessione_aspnet_sso}
```

Il token *SAML* restituito da Cohesion avrà la forma seguente:

```xml
<Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
	<SignedInfo>
		<CanonicalizationMethod Algorithm="http://www.w3.org/TR/2001/REC-xml-c14n-20010315"/>
		<SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
		<Reference URI="#SignedXmlId">
			<DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
			<DigestValue>rNHSy/TcTBIweMerIO3jizVm7u0=</DigestValue>
		</Reference>
	</SignedInfo>
	<SignatureValue>
		Oz9wACp9Rxo4Z4pC8o61iXuVSJNO+UcszPNcAMNFUAY0I17IeGnB93JpwEWGbPfqn5a+i3IQdNsqlIbQtwAGovBQ6DTTb85kDo4YDue26o7a4np5DfwnO6SuhHsBHMbTvnI9A+45Q7Li4On+eELEQWFRrbS11MmwRYBxR5m5PWKupkX3O2r9JtG6tuKwi/L5+A4U/MqLKDdUm9/TX1lzn5c23XpQgiYpkGrwRqp+Q1Dsem7FcV868VYvgwYkRkj/jNgLFtcdoFij+qBRZi47EUa35KeZgP5V1ENcYigG0OXQQbdavCnPcE3EHjEL3ApkuPNVuSbJZv5TjgFdCpqjXg==
	</SignatureValue>
	<KeyInfo>
		<X509Data>
			<X509Certificate>
				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
			</X509Certificate>
		</X509Data>
	</KeyInfo>
	<Object Id="SignedXmlId">
		<profile timestamp="Tue, 25 Jan 2022 22:02:49 GMT" xmlns="">
			<base>
				<placeOfBirth>F051</placeOfBirth>
				<localita_nascita>ROMA</localita_nascita>
				<address>Via dei matti 28 60044 ANCONA AN</address>
				<indirizzo_residenza>Via dei matti 28 60044 ANCONA AN</indirizzo_residenza>
				<gender>M</gender>
				<sesso>M</sesso>
				<digitalAddress>xxxxxxxxxxx@pec.it</digitalAddress>
				<email_certificata>xxxxxxxxxxx@pec.it</email_certificata>
				<familyName>ROSSI</familyName>
				<cognome>ROSSI</cognome>
				<name>MARIO</name>
				<nome>MARIO</nome>
				<dateOfBirth>1985-04-18</dateOfBirth>
				<data_nascita>18/04/1985</data_nascita>
				<spidCode>NAMI0002653481</spidCode>
				<countyOfBirth>RM</countyOfBirth>
				<provincia_nascita>RM/provincia_nascita>
				<fiscalNumber>TINIT-RSSMRA85D18F051Y</fiscalNumber>
				<codice_fiscale>RSSMRA85D18F051Y</codice_fiscale>
				<email>mario.rossi@regione.marche.it</email>
				<companyFiscalNumber/>
				<tipo_autenticazione>PIN</tipo_autenticazione>
				<login>RSSMRA85D18F051Y</login>
				<samlResponseBase64>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgi...</samlResponseBase64>
			</base>
		</profile>
	</Object>
```

Gli attributi dell'utente sono contenuti nell'oggetto *Object*. Gli attributi restituiti possono variare in base all'Identity Provider, al livello di autenticazione e dalle impostazioni dell'utente.

**NOTA BENE**: *Object* contiene anche il token SAML in `samlResponseBase64` il cui valore è quello della _SAMLResponse_ originale restituita dall'Identity Provider scelto dall'utente.


## **Come effettuare il logout?**

Per effettuare il *logout* basterà semplicemente chiamare lo stesso endpoint utilizzato per ottenere gli attributi dell'utente andando a modificare il parametro **Operation** e specificando *LogoutSito* invece di *GetCredential*

Nel nostro esempio

```xml
https://cohesion2.regione.marche.it/SPManager/webCheckSessionSSO.aspx?Operation=LogoutSito&IdSessioneSSO=A5D64899EBCEE1FA6B3E8FB3D3D358D7E5843EA25EDFD62277CACE8A5EF23B1C5E6FAB916F333B0AFB0FAE6C4CDF7B8E8DEE461E8661DB04F5FFB01BC0EF9FCB90EEA2ACA7988119EFE17D5C3BB58BC890D75D724D434D7D60C2F451F6A51D1DFFA900EF0AAA631A51458860C6CEC7B1EBC525561624208066DF276314F41F6EC258CA3F&IdSessioneASPNET=ufrq00x4f0cq2yog2ozv4lxx;https://idp.namirialtsp.com/idp;https://q-marche.turitweb.it/LogOff;AAdzZWNyZXQxGLSQhZFb4TSPKKPNCGfO5MaCcJ99DiuEnIzpO4YvtOrgf+6qvwyEVOguFDCVbyPcZ2hhjbBtPtuCWUGs+FYX9zREfvZ1heshK7yzE24MU7JsIBK17atMdbL31QbDPNKbH2wiswXhL2CJGjY=;AAdzZWNyZXQxGLSQhZFb4TSPKKPNCGfO5MaCcJ99DiuEnIzpO4YvtOrgf+6qvwyEVOguFDCVbyPcZ2hhjbBtPtuCWUGs+FYX9zREfvZ1heshK7yzE24MU7JsIBK17atMdbL31QbDPNKbH2wiswXhL2CJGjY=;

```
