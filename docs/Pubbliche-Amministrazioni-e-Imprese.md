# **Come integro CohesionID?**

## **Richiesta di Integrazione**
Il primo step del processo di _Integrazione_ del Sistema Cohesion nel proprio applicativo è la **Richiesta di Integrazione**.

Nel caso in cui il sistema informativo in questione sia pubblicato da o per conto di Regione Marche è possibile compilare direttamente la **Richiesta di Integrazione** al link: [Richiesta di Integrazione](https://procedimenti.regione.marche.it/Pratiche/Avvia/3049). 
Ricordiamo che Cohesion è integrabile esclusivamente nei siti della pubblica amministrazione. 


### **Livello di autenticazione minimo**

All'interno del modulo di richiesta è necesario specificare il **livello di accesso minimo necessario** per il servizio.
Nel caso in cui il sistema di autenticazione riceva una richiesta di autenticazione per il servizio associata a un livello di autenticazione **inferiore** a quello minimo indicato, **Cohesion ignorerà** il livello di accesso indicato ed esporrà il livello minimo.

#### Esempio concreto

**Ipotesi:** nella richiesta di federazione per il sistema `serviziopubblico.it` è stato specificato un livello di accesso minimo "**Significativo**".

Una volta che il servizio è stato federato, **Cohesion riceve una richiesta di autenticazione con livello "**Basso**"** (quindi inferiore a "Significativo").  
In questo caso, **Cohesion esporrà all’utente una maschera di accesso con livello "Significativo"**, ignorando la richiesta specifica.

---

#### Nota

Per ogni servizio per cui si richiede un livello minimo "**Basso**", è necessario **specificare la motivazione** di tale scelta nel campo di testo sottostante.

Per identificare il livello di autenticazione minimo adeguato si può fare riferimento a quanto indicato nella documentazione tecnica del servizio:  
🔗 [cohesion.regione.marche.it/CohesionDocs/Linee-guida-Livello-di-autenticazione](https://cohesion.regione.marche.it/CohesionDocs/Linee-guida-Livello-di-autenticazione)

## **Convenzione per l'utilizzo di Cohesion Id**

Nel caso in cui l’Ente responsabile non sia Regione Marche è necessario assicurarsi che questi abbia sottoscritto la convenzione per l’utilizzo dell’infrastruttura di autenticazione e accesso della Regione Marche quale Soggetto Aggregatore SPID:
[https://procedimenti.regione.marche.it/AreaPA/TipologieProcedimento/DettagliTer/13915](https://procedimenti.regione.marche.it/AreaPA/TipologieProcedimento/DettagliTer/13915)


## **ID Sito**

Effettuata la richiesta, i nostri tecnici processeranno i dati inseriti ed invieranno una mail al tecnico indicato con l'**ID Sito** univoco per la vostra applicazione.

## **Integrazione**

Completata la richiesta di Integrazione e ottenuto l'ID Sito, sarà possibile procedere all'effettiva integrazione nel proprio applicativo.
Per supporto nell'integrazione, consulta la documentazione relativa al [flusso di autenticazione](/CohesionDocs/Flusso-di-Autenticazione) e i [parametri della richiesta di autenticazione](/CohesionDocs/Parametri-della-Richiesta-di-Autenticazione). Nel caso si voglia utilizzare una delle integrazioni già disponibili, consultare la [Documentazione Tecnica](/CohesionDocs/Documentazione-tecnica)
