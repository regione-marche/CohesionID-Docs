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
