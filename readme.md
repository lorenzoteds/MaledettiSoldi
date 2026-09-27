# 🐷 MaledettiSoldi 💸

> **Un'applicazione web leggera, condivisa e colorata per gestire le spese familiari di coppia, senza abbonamenti e con i tuoi dati su cloud privato.**

![MaledettiSoldi Preview](icon-192.png)

---

## ✨ Funzionalità

- 👥 **Due Colonne Dedicate:** Visualizzazione separata delle spese per i due membri della famiglia (es. Veronica e Lorenzo), con subtotali dedicati.
- 📅 **Navigazione Mensile:** Spostati tra i mesi passati e futuri con frecce rapide e vedi subito entrate, uscite e saldo netto.
- 📊 **Grafici a Torta (Chart.js):** Ripartizione visiva per categoria di spesa del mese in corso.
- 📥 **Export CSV / Excel:** Scarica in qualsiasi momento lo storico completo o il resoconto del singolo mese.
- 📱 **PWA Installabile:** Salvabile su schermata home di Android e iPhone come applicazione a schermo intero senza barre del browser.
- 🔐 **Sicurezza & Privacy:** Autenticazione con email e password gestita tramite Supabase e Row Level Security (RLS).
- 🎨 **Interfaccia Pastel & Simpatica:** Palette colori morbida, icone tematiche per categoria e design mobile-first.

---

## 🗂️ Struttura del Progetto

Il progetto è volutamente minimale e non richiede compilatori o bundler (Node/npm):

```text
├── index.html        # Frontend completo (HTML, CSS e JavaScript con Supabase SDK)
├── manifest.json     # File di configurazione Web App per l'installazione su mobile
├── icon-192.png      # Icona PWA formato standard (192x192)
├── icon-512.png      # Icona PWA alta risoluzione (512x512)
└── README.md         # Documentazione del progetto
```

---

## 🚀 Guida all'Installazione & Configurazione

Per usare l'app con il proprio database personale:

### 1. Crea il Database su Supabase (Gratuito)

1. Iscriviti su [supabase.com](https://supabase.com) e crea un nuovo progetto (es. `MaledettiSoldi`).
2. Vai su **SQL Editor** ed esegui questa query per creare la tabella:

```sql
CREATE TABLE family_expenses (
    id BIGSERIAL PRIMARY KEY,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    date DATE NOT NULL DEFAULT CURRENT_DATE,
    type VARCHAR(10) NOT NULL CHECK (type IN ('income', 'expense')),
    category VARCHAR(50) NOT NULL,
    amount NUMERIC(10, 2) NOT NULL CHECK (amount > 0),
    description TEXT,
    author VARCHAR(50) NOT NULL
);

-- Attiva la sicurezza RLS
ALTER TABLE family_expenses ENABLE ROW LEVEL SECURITY;

-- Permetti le operazioni agli utenti che effettuano il login
CREATE POLICY "Accesso utenti autenticati" 
ON family_expenses 
FOR ALL 
TO authenticated 
USING (true) 
WITH CHECK (true);
```

3. Vai su **Authentication** -> **Users** -> **Add user** -> **Create user** e imposta l'email e la password per la famiglia.
4. In **Authentication** -> **Providers** -> **Email**, disattiva l'opzione **Confirm email** per permettere il login immediato.

---

### 2. Configura le Chiavi nell'`index.html`

Apri `index.html` e inserisci il tuo URL e la tua chiave pubblica (`anon key`) che trovi nelle impostazioni API del tuo progetto Supabase:

```javascript
const SUPABASE_URL = 'https://TUO-PROGETTO.supabase.co';
const SUPABASE_ANON_KEY = 'LA_TUA_CHIAVE_ANON_PUBBLICA';
```

*(Opzionale: nel file `index.html` puoi rinominare i nomi nei menu e nelle colonne con quelli che preferisci).*

---

### 3. Pubblica su GitHub Pages

1. Carica tutti i file nella tua repository GitHub (anche privata).
2. Vai su **Settings** -> **Pages**.
3. Sotto **Build and deployment**, seleziona il branch `main` (o `master`) e la cartella `/ (root)`, poi salva.
4. Dopo pochi istanti, GitHub genererà il link pubblico protetto da HTTPS.

---

### 4. Installazione su Smartphone

1. Apri il link dal browser dello smartphone (Chrome su Android o Safari su iOS).
2. Effettua il login con le credenziali create su Supabase.
3. Seleziona dal menu del browser:
   - **Android (Chrome):** `Installa app` oppure `Aggiungi a schermata Home`.
   - **iOS (Safari):** Tasto Condividi ⎋ -> `Aggiungi alla schermata Home`.

---

## 🛠️ Tecnologie Utilizzate

- **HTML5 / Vanilla JavaScript (ES6+)**
- **Chart.js:** Per i grafici di ripartizione spese
- **Supabase JS Client v2:** Autenticazione e database cloud PostgreSQL
- **Google Fonts:** Comfortaa & Nunito