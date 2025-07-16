# **FAQ – Domande frequenti**

---

## 🧭 Informazioni generali

### **Che cos'è Cohesion ID?**
Cohesion ID è un framework di autenticazione progettato per verificare l'identità degli utenti (persone fisiche con codice fiscale) che accedono ai servizi digitali della Regione Marche o degli Enti convenzionati.

### **Quali sono i livelli di autenticazione supportati da Cohesion?**
Cohesion supporta tre livelli di autenticazione: basso, significativo ed elevato, offrendo opzioni di sicurezza adatte a diverse esigenze.

### **Quali sono i sistemi di autenticazione supportati da Cohesion?**
Cohesion supporta i principali sistemi di autenticazione e identificazione digitale previsti dal Codice dell'Amministrazione Digitale, tra cui SPID, Carta di Identità Elettronica (CIE-ID), CNS e nodo italiano eIDAS.

---

## 🧾 Gestione credenziali

### **Come posso ottenere credenziali di accesso Cohesion id?**
!!! warning "Attenzione"
    A partire dal **1° marzo 2021**, ai sensi dell’art. 24 del D.L. 76/2020, convertito con modificazioni dalla L. 120/2020 (*Decreto Semplificazioni*), le **credenziali Cohesion id possono essere rilasciate esclusivamente al personale interno della Pubblica Amministrazione**.

    I **cittadini non possono più ottenere nuove credenziali Cohesion ID**, ma possono accedere ai servizi digitali tramite:

    - **SPID** (Sistema Pubblico di Identità Digitale)  
    - **CIE (CieID)** – Carta d’Identità Elettronica  
    - **CNS** – Carta Nazionale dei Servizi  

Le credenziali Cohesion id possono essere rilasciate **esclusivamente ai dipendenti della Pubblica Amministrazione** o a soggetti autorizzati all’interno di enti convenzionati, nei casi previsti dalla normativa vigente.

Per effettuare la richiesta, è possibile rivolgersi a:

- **Servizio Cittadinanza Digitale – Regione Marche**  
  📧 [cittadinanza.digitale@regione.marche.it](mailto:cittadinanza.digitale@regione.marche.it)

- **Ufficio di registrazione del proprio ente di appartenenza**  
  *(l’elenco degli uffici abilitati è attualmente in fase di aggiornamento)*


### **Ho dimenticato la password, come posso recuperarla?**
In caso di smarrimento della password, è possibile avviare la procedura di recupero online:  
👉 [Recupera la password](https://cohesion2.regione.marche.it/MyCohesion/ResetPassword)

In alternativa, è possibile cliccare su **My Cohesion**, il menu disponibile in alto a destra in ogni pagina di login Cohesion id, e selezionare la voce **Reset Password** per avviare la procedura.

### **Come posso cambiare la mia password?**

Per modificare la propria password, accedere al menu **My Cohesion** presente in alto a destra in ogni pagina di login e selezionare la voce **Cambia Password**.

Per procedere sarà necessario effettuare l’autenticazione.

### **Ho dimenticato il PIN, come posso recuperarlo?**
Il recupero del PIN può essere richiesto:

- Telefonando all’**Helpdesk**: 071 8063000, interno 1
- Inviando una mail a: [cittadinanza.digitale@regione.marche.it](mailto:cittadinanza.digitale@regione.marche.it)  
- Presso un **ufficio di registrazione abilitato**

### **Come posso aggiungere il PIN alla mia utenza esistente (Codice Fiscale + Password)?**
Gli utenti in possesso delle sole credenziali Codice Fiscale + Password possono richiedere l’aggiunta del PIN, necessario per accedere ai servizi che richiedono un livello di autenticazione significativo, **purché rientrino nei casi previsti dalla normativa vigente **(***vedi riquadro “Attenzione” all’inizio della pagina***).

La richiesta può essere effettuata attraverso uno dei seguenti canali:

- Contattando l’**Helpdesk**:
☎️ 071 8063000, interno 1

- Scrivendo a:
📧 [cittadinanza.digitale@regione.marche.it](mailto:cittadinanza.digitale@regione.marche.it)
- Rivolgendosi a un ufficio di registrazione abilitato (lista in aggiornamento)
---

## ⚙️ Integrazione e configurazione

### **Come posso ottenere il mio ID Sito?**
Se il sistema informativo che vuoi integrare è pubblicato da o per conto di Regione Marche, è possibile compilare direttamente la richiesta di integrazione al seguente link:  
🔗 [https://procedimenti.regione.marche.it/Pratiche/Avvia/3049](https://procedimenti.regione.marche.it/Pratiche/Avvia/3049)

Effettuata la richiesta, i nostri tecnici forniranno il vostro codice ID Sito univoco.

Nel caso in cui l'ente responsabile sia differente, è necessario assicurarsi che questi abbia sottoscritto la convenzione per l'utilizzo dell'Infrastruttura di autenticazione e accesso della Regione Marche in qualità di soggetto aggregatore SPID:  
🔗 [https://procedimenti.regione.marche.it/AreaPA/TipologieProcedimento/DettagliTer/13915](https://procedimenti.regione.marche.it/AreaPA/TipologieProcedimento/DettagliTer/13915)

### **Ho bisogno di assistenza per l'integrazione di Cohesion nel mio applicativo, chi posso contattare?**
Se hai bisogno di aiuto in merito all'integrazione di Cohesion, contattaci all'indirizzo [Integrazione Cohesion](mailto:integrazionecohesion@regione.marche.it)

---

## 🔐 Livelli di autenticazione

### **Un sito della Pubblica Amministrazione, esposto solo su rete interna e utilizzato esclusivamente da dipendenti, può adottare un livello di autenticazione "basso"?**
**No**, in linea generale un sito della Pubblica Amministrazione, anche se **accessibile solo da rete interna** e **riservato ai soli dipendenti**, **non può adottare un livello di autenticazione "basso"** se sussiste almeno una delle seguenti condizioni:

- tratta dati personali o informazioni riservate;
- consente la gestione o visualizzazione di contenuti amministrativi, come **atti, determinazioni, provvedimenti o documenti protocollati**;
- abilita operazioni rilevanti dal punto di vista amministrativo o organizzativo.

L’adozione di un livello di autenticazione basso è ammessa **solo se sussistono contemporaneamente tutte le seguenti condizioni**:

- Il sito **non tratta dati personali, né atti amministrativi o documenti protocollati**;
- Le funzionalità offerte hanno un **basso impatto sul rischio di sicurezza**, limitandosi ad attività informative o consultive non critiche;
- L’accesso è **protetto a livello infrastrutturale**, ad esempio tramite **rete isolata, VPN o autenticazione a dominio**;
- È garantito il rispetto dei principi di **proporzionalità, minimizzazione e necessità**, ai sensi del Regolamento (UE) 2016/679 – GDPR.

In assenza di queste condizioni, il livello minimo di autenticazione richiesto deve essere almeno **"significativo"**, come previsto dalla normativa.

### **Gli utenti del servizio PA hanno sempre utilizzato nome utente e password di Cohesion (livello "basso") per accedere.Innalzando il livello a "significativo" (Codice Fiscale, Password e PIN), molti di loro non riusciranno più ad accedere. Come è possibile fare?**
Con l’innalzamento del livello di autenticazione da **"basso" a "significativo"**, l’accesso al servizio richiede un meccanismo di autenticazione più robusto, conforme al **Regolamento eIDAS** e alle **Linee guida AgID sull’identità digitale**.

Per garantire la continuità del servizio:

- **Gli utenti esterni alla Pubblica Amministrazione** (es. cittadini, imprese, professionisti) dovranno utilizzare un’identità digitale qualificata, scegliendo tra:
  - **SPID** (Sistema Pubblico di Identità Digitale)  
  - **CIE** (Carta d’Identità Elettronica)
  - **CNS** (Carta Nazionale dei Servizi)

  Questi strumenti garantiscono un livello di autenticazione **"significativo" o "elevato"**, pienamente compatibile con il nuovo requisito del servizio.

- **Per gli utenti interni alla PA (dipendenti)**, in alternativa a SPID/CIE/CNS, **è possibile rilasciare un PIN Cohesion**, che consente l’accesso al sistema in modalità **"significativa"** attraverso il circuito di autenticazione federato Cohesion, integrando:
  - **Codice Fiscale**
  - **Password**
  - **PIN personale**

In questo modo si garantisce il rispetto dei requisiti normativi senza compromettere l’accessibilità al servizio da parte degli utenti autorizzati.

### **C’è un livello minimo di autenticazione?**
**Sì**, secondo le normative in vigore, il livello minimo di autenticazione richiesto è **"significativo" (livello 2)**.

Tuttavia, se il servizio non richiede una protezione elevata e soddisfa determinati requisiti (ad esempio, non trattando dati sensibili o riservati), può essere richiesto un livello di autenticazione **inferiore**. In tal caso, è necessario giustificare questa scelta in fase di integrazione o, successivamente, attraverso una richiesta formale di modifica del livello di autenticazione.

### **Un sito che gestisco è già integrato in Cohesion, che di default applica il livello "significativo" di autenticazione. Tuttavia, il mio sito ha caratteristiche che giustificherebbero un accesso con livello "basso". A chi posso comunicarlo?**
Nel sistema Cohesion, il livello di autenticazione **"significativo"** viene applicato come impostazione predefinita per garantire il rispetto delle Linee guida AgID. Tuttavia, se il sito **non tratta dati personali, atti amministrativi o contenuti sensibili** e soddisfa i requisiti per un livello di autenticazione **"basso"**, è possibile richiedere l’adeguamento.

La **comunicazione del livello minimo di autenticazione** avviene in due modalità:

- **In fase di federazione iniziale**:  
  È necessario compilare il **modulo di richiesta federazione**, specificando il livello minimo desiderato (il livello basso deve essere opportunamente giustificato), disponibile al seguente link:  
  🔗 [Modulo federazione Cohesion – Avvio](https://procedimenti.regione.marche.it/Pratiche/Avvia/3049)

- **In caso di variazione successiva** (es. abbassamento del livello da "significativo" a "basso"):  
  Occorre compilare il **modulo di richiesta di modifica del livello di autenticazione**, disponibile qui:  
  🔗 [Modulo richiesta modifica livello di accesso](https://procedimenti.regione.marche.it/Pratiche/Avvia/14234)

La richiesta sarà valutata dal team tecnico e, se conforme ai criteri di sicurezza e proporzionalità previsti dalla normativa vigente, il livello potrà essere aggiornato di conseguenza nel sistema Cohesion.

---
