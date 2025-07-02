# 🏛️ Linea Guida per l'Assegnazione del Livello di Accesso (LoA) ai Servizi Digitali

---

## ⚖️ 1. Riferimenti normativi

- **Regolamento (UE) n. 910/2014 (eIDAS)**: definisce i livelli di garanzia delle identità elettroniche (basso, sostanziale, alto).
- **D.Lgs. 82/2005 (CAD)**, artt. 64, 64-bis, 65.
- **Linee guida AgID su SPID, CIE e CNS**.
- **Principio di proporzionalità (CAD e art. 5 GDPR)**: il livello di autenticazione richiesto deve essere commisurato alla sensibilità del servizio.

---

## 🧩 2. Definizione dei Livelli di Accesso

| LoA | Descrizione | Esempi di credenziali accettate |
|-----|-------------|-------------------------------|
| **Basso** | Identità con verifica minima, spesso senza garanzia sulla persona fisica | SPID Livello 1, social login, registrazione via email |
| **Sostanziale** | Identità verificata con almeno due fattori e rilascio da soggetto accreditato | SPID Livello 2, CIE con app/OTP, CNS con PIN |
| **Alto** | Identità digitale con alto grado di certezza, uso di certificati o dispositivi hardware | SPID Livello 3, CIE con lettore + PIN, CNS con autenticazione forte |

---

## 🧮 3. Criteri per l’assegnazione del LoA

| Criterio | LoA basso | LoA sostanziale | LoA alto |
|---------|-----------|----------------|----------|
| **Accesso a dati personali non sensibili** | ✅ | ✅ | ✅ |
| **Accesso a dati sensibili / sanitari / patrimoniali** | ❌ | ✅ | ✅ |
| **Servizio informativo senza valore legale** | ✅ | ❌ | ❌ |
| **Servizio che produce effetti giuridici o dichiarazioni** | ❌ | ✅ | ✅ |
| **Servizio con firma elettronica avanzata o qualificata** | ❌ | ❌ | ✅ |
| **Servizio con valore probatorio o patrimoniale rilevante** | ❌ | ✅ | ✅ |
| **Servizio transfrontaliero (eIDAS)** | ❌ | ✅ | ✅ (se richiesto) |

---

## 🗂️ 4. Esempi di Servizi Regione Marche per ciascun LoA

| Servizio | LoA richiesto | Note |
|----------|---------------|------|
| Consultazione bandi pubblici | **Basso** | Nessun dato personale richiesto |
| Prenotazione evento informativo | **Basso** | Autenticazione opzionale |
| Consultazione fascicolo sanitario elettronico (FSE) | **Sostanziale** | Dati sensibili |
| Richiesta di contributo economico (es. bandi PSR) | **Sostanziale** | Rilevanza giuridico-economica |
| Firma digitale su istanza SUAP o SCIA | **Sostanziale** | Comporta effetti legali e responsabilità |
| Accesso al portale della formazione professionale | **Sostanziale** | Dati personali + attività tracciate |
| Invio segnalazione non firmata (es. decoro urbano) | **Basso** | Nessuna responsabilità formale dell’utente |

---

## 🛡️ 5. Obblighi per le strutture regionali

- Ogni servizio digitale **deve dichiarare il livello di autenticazione richiesto** nella [richiesta di federazione a Cohesion Id](https://procedimenti.regione.marche.it/Pratiche/Avvia/3049).
- I sistemi esposti ai cittadini devono **accettare SPID, CIE e CNS** come previsto dall’art. 64 CAD.
- È vietato imporre un livello di accesso **superiore a quello necessario**.

## FAQ
### ❓ Domanda:
Un sito della Pubblica Amministrazione, esposto solo su rete interna e utilizzato esclusivamente da dipendenti, può adottare un livello di autenticazione "basso"?

### ❌ Risposta:
**No**, in linea generale un sito della Pubblica Amministrazione, anche se **accessibile solo da rete interna** e **riservato ai soli dipendenti**, **non può adottare un livello di autenticazione "basso"** se sussiste almeno una delle seguenti condizioni:

- tratta **dati personali** o **informazioni riservate**;
- consente la gestione o visualizzazione di **contenuti amministrativi**, come **atti, determinazioni, provvedimenti o documenti protocollati**;
- abilita **operazioni rilevanti dal punto di vista amministrativo o organizzativo**.

L’adozione di un livello di autenticazione **basso** è ammessa **solo se sussistono contemporaneamente tutte le seguenti condizioni**:

- Il sito **non tratta dati personali, né atti amministrativi o documenti protocollati**;
- Le funzionalità offerte hanno un **basso impatto sul rischio di sicurezza**, limitandosi ad attività informative o consultive non critiche;
- L’accesso è **protetto a livello infrastrutturale**, ad esempio tramite **rete isolata, VPN o autenticazione a dominio**;
- È garantito il rispetto dei principi di **proporzionalità, minimizzazione e necessità**, ai sensi del **Regolamento (UE) 2016/679 – GDPR**.

In assenza di queste condizioni, il livello minimo di autenticazione richiesto deve essere almeno **"significativo"**, come previsto dalla normativa.

### ❓ Domanda:
Gli utenti del servizio PA hanno sempre utilizzato nome utente e password di Cohesion (livello "basso") per accedere. Innalzando il livello a "significativo" (Codice Fiscale, Password e PIN), molti di loro non riusciranno più ad accedere. Come è possibile fare?

### ✅ Risposta:
Con l’innalzamento del livello di autenticazione da **"basso" a "significativo"**, l’accesso al servizio richiede un **meccanismo di autenticazione più robusto**, conforme al **Regolamento eIDAS** e alle **Linee guida AgID sull’identità digitale**.

Per garantire la continuità del servizio:

- **Gli utenti esterni alla Pubblica Amministrazione** (es. cittadini, imprese, professionisti) **dovranno utilizzare un’identità digitale qualificata**, scegliendo tra:
  - **SPID (Sistema Pubblico di Identità Digitale)**  
  - **CIE (Carta d’Identità Elettronica)**  
  - **CNS (Carta Nazionale dei Servizi)**  

  Questi strumenti garantiscono un livello di autenticazione **"significativo" o "elevato"**, pienamente compatibile con il nuovo requisito del servizio.

- **Per gli utenti interni alla PA (dipendenti)**, in alternativa a SPID/CIE/CNS, **è possibile rilasciare un PIN Cohesion**, che consente l’accesso al sistema in modalità **"significativa"** attraverso il circuito di autenticazione federato Cohesion, integrando:
  - **Codice Fiscale**
  - **Password**
  - **PIN personale**

In questo modo si garantisce il rispetto dei requisiti normativi senza compromettere l’accessibilità al servizio da parte degli utenti autorizzati.


### ❓ Domanda:
Un sito che gestisco è già integrato in Cohesion, che di default applica il livello "significativo" di autenticazione. Tuttavia, il mio sito ha caratteristiche che giustificherebbero un accesso con livello "basso". A chi posso comunicarlo?

### ✅ Risposta:
Nel sistema Cohesion, il livello di autenticazione **"significativo"** viene applicato come impostazione predefinita per garantire il rispetto delle Linee guida AgID. Tuttavia, se il sito **non tratta dati personali, atti amministrativi o contenuti sensibili** e soddisfa i requisiti per un livello di autenticazione **"basso"**, è possibile richiedere l’adeguamento.

La **comunicazione del livello minimo di autenticazione** avviene in due modalità:

- **In fase di federazione iniziale**:  
  È necessario compilare il **modulo di richiesta federazione**, specificando il livello minimo desiderato (il livello basso deve essere opportunamente giustificato), disponibile al seguente link:  
  🔗 [Modulo federazione Cohesion – Avvio](https://procedimenti.regione.marche.it/Pratiche/Avvia/3049)

- **In caso di variazione successiva** (es. abbassamento del livello da "significativo" a "basso"):  
  Occorre compilare il **modulo di richiesta di modifica del livello di autenticazione**, disponibile qui:  
  🔗 [Modulo richiesta modifica livello di accesso](https://procedimenti.regione.marche.it/Pratiche/Avvia/14234)

La richiesta sarà valutata dal team tecnico e, se conforme ai criteri di sicurezza e proporzionalità previsti dalla normativa vigente, il livello potrà essere aggiornato di conseguenza nel sistema Cohesion.


