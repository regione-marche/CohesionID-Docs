# **Come rinnovare il Token**
Il Token di Cohesion ha validità di 60 minuti.
Se si vuole rinnovare il Token è necessario che l'utente transiti nuovamente per la pagina di LogIn di Cohesion. 
Questa operazione va fatta prima della scadenza del Token.

Esempi:

**1) Integrazione C#:**

Per rinnovare il token è sufficiente reindirizzare l'utente alla propria pagina di login, in modo da eseguire nuovamente il codice
```c#
CohesionSSO cohesionSSO = new CohesionSSO(Request, Response, Session);

cohesionSSO.ValidateFE();
```
**2) Integrazione Servlet Java:**

Per rinnovare il token è sufficiente reindirizzare l'utente alla Servlet Java, di norma richiamata dalla pagina di login della applicazione.

In entrambi i casi l'utente viene reindirizzato a Cohesion, il sistema verifica che ha già una sessione valida, genera un nuovo Token valido 60 minuti e reindirizza l'utente all'applicazione.

Se si integra con modalità SPManager, per evitare che - al nuovo passaggio su Cohesion - venga riproposta all'utente la schermata di scelta del provider, impostare il parametro entityID passandolo alla pagina WAYF SAML.

Ad esempio:

* **Cohesion Sviluppo**
```xml
https://cohesion2sviluppo.regione.marche.it/SPManager/WAYF.aspx?entityID=cohesion.regione.marche.it:idp
```
* **Cohesion Produzione**
```xml
https://cohesion2.regione.marche.it/SPManager/WAYF.aspx?entityID=cohesion2.regione.marche.it:idp
```

ovvero passando il parametro entityID valorizzato con l'id dell'entità SAML a cui si vuole redirigere automaticamente (che nel caso di cohesion è cohesion.regione.marche.it:idp).



