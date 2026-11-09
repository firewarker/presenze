# Presenze VVF Lentini — istruzioni

App singola pagina (`index.html`) con login reale Firebase, isolamento per turno e account amministratore.
Già agganciata al tuo progetto **presenze-7ba78** (databaseURL, projectId, authDomain, storageBucket sono corretti).

Ti restano **5 passi**, tutti dalla Console Firebase e da GitHub (niente terminale).

---

## 1 — Configurazione Firebase ✓ già inserita

apiKey, sender e appId sono già dentro `index.html`: **non devi toccare il file**.
La apiKey Web di Firebase non è segreta, è normale che resti nel file pubblicato. La protezione vera arriva dalle regole (passo 4) e dai domini autorizzati (passo 2).

## 2 — Abilita l'accesso email/password

Console Firebase → **Authentication** → **Sign-in method** → abilita **Email/Password** → Salva.

**Importante — domini autorizzati:** sempre in Authentication → **Settings / Impostazioni** → **Domini autorizzati** → **Aggiungi dominio** → inserisci **`firewarker.github.io`**.
Senza questo passo, dal sito pubblicato il login risponde con l'errore `auth/unauthorized-domain`. (`localhost` e `presenze-7ba78.firebaseapp.com` sono già autorizzati di default.)

## 3 — Crea i 5 account

Authentication → **Users** → **Aggiungi utente**, uno per riga (email + password a tua scelta):

| Email | Chi è / cosa vede |
|---|---|
| `admin@vvflentini.it` | **Amministratore** — vede e gestisce tutti i turni (cambia turno dal menù) |
| `turnoa@vvflentini.it` | Solo turno **A** |
| `turnob@vvflentini.it` | Solo turno **B** |
| `turnoc@vvflentini.it` | Solo turno **C** |
| `turnod@vvflentini.it` | Solo turno **D** |

Le email devono essere **esattamente** queste (l'app riconosce il turno dalla parola `turnoa/b/c/d`). Le password le decidi tu e le distribuisci ai capi turno.

## 4 — Pubblica le regole di sicurezza

Console Firebase → **Realtime Database** → scheda **Regole** → incolla **tutto** il contenuto di `database.rules.json` → **Pubblica**.
Sono queste regole a impedire davvero che un turno legga i dati di un altro (il blocco non è solo nella schermata, è nel database).

## 5 — Carica su GitHub e attiva il sito

1. Vai su `https://github.com/firewarker/presenze`.
2. Carica nella radice del repository (Add file → Upload files → Commit) questi **5 file**, tutti insieme:
   - `index.html` (l'app)
   - `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` (servono per installarla sul telefono come un'app, con la sua icona)
3. Settings → **Pages** → Source: **Deploy from a branch** → Branch: `main` / `/root` → Save.
4. Dopo 1–2 minuti l'app sarà online su: **https://firewarker.github.io/presenze/**

(`database.rules.json` e questo `LEGGIMI.md` servono solo a te: puoi caricarli o tenerli da parte.)

## 6 — Primo avvio: inserisci il personale

1. Apri il sito, accedi (es. `admin@vvflentini.it`).
2. Vai in **Impostazioni** e per ogni vigile aggiungi: **qualifica**, **posizione 1–8** (l'ordine del turnario, decide chi va in Recupero) e le **ferie** del primo anno (spettanti + residue dell'anno prima). Dagli anni successivi le ferie si calcolano da sole.
3. Da lì in poi: il **Prospetto mensile** calcola da solo turni e Recuperi, segni a mano solo le assenze (L, M, ecc.), e i tre PDF (presenze giornaliere, ferie, buoni pasto) si generano dai pulsanti.

### Cose da sapere sul personale
- **Modifiche di qualifica o posizione** valgono *dal mese che stai guardando in poi*: i mesi passati (e i PDF già fatti) non cambiano. "Da sempre" serve solo a correggere un errore di inserimento.
- **Chi va via** (trasferito, pensione): usa **Cessato / trasferito**, non "Elimina". Resta nello storico dei mesi in cui ha lavorato e sparisce da quelli dopo.
- **Chi arriva**: aggiungilo stando sul mese di arrivo; compare da quel mese in poi.
- **Codici nuovi** (es. INF infortunio): quando li crei scegli se sono *assenza* (niente buono) o *presenza* (buono pasto sì).

---

## Note

- È la **versione 1**: appena è online e hai inserito un paio di persone, fai un giro veloce (un mese, un PDF, un'assenza). Se qualcosa stona me lo dici e lo sistemo.
- **Cambiare dominio email?** Se non vuoi `@vvflentini.it`, aprilo in due punti e tienili uguali: in `index.html` i valori `ADMIN_EMAIL` ed `EMAIL_DOMAIN`, e in `database.rules.json` l'indirizzo admin e il `+ '@vvflentini.it'`. Le email degli account devono combaciare.
- **Firma sui PDF:** quando generi un documento, l'app chiede quale **Capo Squadra** (CS/CSE/CR) lo sta firmando e ne riporta il nominativo sotto "Il Capo Turno".
- **Turno PROVA (solo admin):** nel menù turni trovi anche `PROVA (test)`. È una sandbox isolata — usa lo stesso turnario reale ma dati separati — quindi puoi fare tutte le prove che vuoi **senza toccare A/B/C/D**. I test sui turni veri, invece, **sì** che si riflettono sulle schermate di quel turno.
- **Cambio password:** ogni utente (turni e admin) può cambiarla da **Impostazioni → Account → Cambia password** (chiede la password attuale per sicurezza).
- **Dove si salvano i PDF:** su computer con **Chrome o Edge** si apre la finestra *Salva con nome*, dove scegli la cartella. Sul **telefono** si apre il menù *Condividi* (Salva su File, WhatsApp, Mail…). Su **Firefox/Safari da computer** il file va nei Download: per scegliere la cartella attiva nel browser l'opzione *"Chiedi sempre dove salvare i file"*.
- **Backup:** in **Impostazioni → Backup → Esegui backup** salvi un file `.json` con tutto (personale, codici, mezzi, tutti i mesi di tutti gli anni). L'admin salva tutti i turni, un turno salva solo il proprio. Consiglio: uno al mese, conservato in due posti (PC + chiavetta o cloud).
- **Ripristinare un backup (solo in emergenza):** Console Firebase → **Realtime Database** → Dati.
  - Backup dell'admin (*tutti-i-turni*): seleziona il nodo **`turni`** → menù ⋮ → **Importa JSON** → scegli il file.
  - Backup di un turno (*turnoD*…): apri `turni` → seleziona il nodo del turno (es. **`D`**) → ⋮ → **Importa JSON**.
  - Attenzione: l'importazione **sostituisce** quello che c'è in quel nodo con il contenuto del file.
- **Salto turno:** sopra i giorni del Prospetto mensile (e nei PDF ferie e buoni) c'è la riga **Salto turno**: una casella per ogni coppia diurno+notturno con il salto di quei due giorni (es. *Salto D2*). È calcolata dal turnario, per tutti i turni e tutti gli anni.
- **Tema:** di giorno è color carta, chiaro e riposante; di notte diventa scuro da solo se il telefono/PC è in modalità scura. Il pulsante tondo accanto a "Esci" cambia tra *automatico → chiaro → scuro*, e ognuno se lo ritrova come l'ha lasciato.
- **Installarla sul telefono:** apri il sito, poi dal menù del browser scegli **"Aggiungi a schermata Home"** (Safari: tasto Condividi). Compare l'icona VVF e si apre a tutto schermo come un'app.
- **Connessione:** in alto a destra vedi **Online** (verde), **Salvataggio…** (giallo) oppure **Offline** (rosso). Se sei offline le modifiche restano in attesa: non chiudere la pagina finché non torna "Online" (se provi a chiuderla l'app ti avvisa).
