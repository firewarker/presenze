# Presenze VVF Lentini — istruzioni

App singola pagina (`index.html`) collegata al progetto Firebase **presenze-7ba78**.

**Come si entra:** la prima volta su ogni telefono/PC si scrive il **codice del distaccamento**; poi l'app si apre direttamente sulla scelta del turno (A, B, C, D). L'**amministratore** entra a parte con la propria email e password.

---

## 1 — Configurazione Firebase ✓ già inserita

apiKey, sender e appId sono già dentro `index.html`: **non devi toccare il file**. La apiKey Web di Firebase non è segreta; la protezione vera sono le regole del database (passo 4).

## 2 — Accesso email/password e dominio (già fatto)

Authentication → **Metodo di accesso** → **Email/Password** abilitato, e in **Impostazioni → Domini autorizzati** c'è **`firewarker.github.io`** (senza, il login dal sito dà `auth/unauthorized-domain`).

## 3 — Account: crea quello del distaccamento

Authentication → **Utenti** → **Aggiungi utente**:

| Email | Password | A cosa serve |
|---|---|---|
| `distaccamento@vvflentini.it` | `VVF-Lentini-tuttupessu` | L'accesso di **tutti i turni**. I colleghi scrivono solo **tuttupessu**: l'app aggiunge da sola `VVF-Lentini-` davanti. Maiuscole e spazi non contano ("Tuttu Pessu" va bene). Dopo **3 tentativi sbagliati** compare il suggerimento *"cosa diceva sempre Tano?"*. |
| `luca.lagana@vigilfuoco.it` | *(la tua, già creata)* | **Amministratore**: tutti i turni + turno PROVA + backup completo. |

Poi, **solo dopo** aver fatto i passi 4 e 5, **elimina** i vecchi account, che non servono più: `turnoa@`, `turnob@`, `turnoc@`, `turnod@` e `admin@vvflentini.it` (menù ⋮ a destra di ogni riga → *Elimina account*). I dati **non** si perdono: sono nel database, non negli account.

**Cambiare il codice in futuro** (es. se gira tra chi non dovrebbe): elimina l'account `distaccamento@vvflentini.it` e ricrealo con password `VVF-Lentini-` + nuovo codice **tutto minuscolo e senza spazi** (es. `VVF-Lentini-nuovocodice`). Se cambi codice, cambia anche il suggerimento su Tano: me lo dici e lo aggiorno. Ogni dispositivo richiederà il nuovo codice una volta, con il messaggio *"Il codice del distaccamento è cambiato"*. L'app non va toccata.

**Password admin dimenticata:** nella finestra *accesso amministratore* scrivi la tua email e premi **Password dimenticata?**: ti arriva il link per sceglierne una nuova (la tua email è reale).

## 4 — Pubblica le regole di sicurezza (nuove)

Console Firebase → **Realtime Database** → scheda **Regole** → cancella tutto, incolla **tutto** il contenuto del nuovo `database.rules.json` → **Pubblica**.
Le regole lasciano entrare solo l'account del distaccamento (turni A, B, C, D) e il tuo (tutto, compreso PROVA). La tua email compare solo qui, **non** nella pagina pubblica.

**Ordine consigliato:** prima crea l'account del distaccamento (passo 3), poi pubblica le regole, poi carica il nuovo `index.html` su GitHub (passo 5). Elimina i vecchi account solo alla fine.

## 5 — Carica su GitHub e attiva il sito

1. Vai su `https://github.com/firewarker/presenze`.
2. Carica nella radice del repository (Add file → Upload files → Commit) questi **5 file**, tutti insieme:
   - `index.html` (l'app)
   - `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` (servono per installarla sul telefono come un'app, con la sua icona)
3. Settings → **Pages** → Source: **Deploy from a branch** → Branch: `main` / `/root` → Save.
4. Dopo 1–2 minuti l'app sarà online su: **https://firewarker.github.io/presenze/**

(`database.rules.json` e questo `LEGGIMI.md` servono solo a te: puoi caricarli o tenerli da parte.)

## 6 — Primo avvio: inserisci il personale

1. Apri il sito, inserisci il codice e scegli il turno (oppure entra come amministratore).
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
- **Cambia turno:** il pulsante giallo in alto a destra riporta alla scelta del turno, per guardare (o compilare) un altro turno. Con **"Ricorda la scelta"** spuntato, quel dispositivo si apre direttamente sull'ultimo turno scelto.
- **Dimentica questo dispositivo:** in **Impostazioni → Accesso**. Utile su un PC condiviso o se si cambia telefono: alla prossima apertura servirà di nuovo il codice.
- **Firma sui PDF:** quando generi un documento l'app chiede chi lo firma. In cima ci sono i **capi squadra** (CS/CSE/CR); sotto, *"In assenza del capo squadra"*, gli altri vigili. Per il PDF del giorno compaiono solo i **presenti** quel giorno. Se firma un capo squadra il PDF riporta *"Il Capo Turno"*, se firma un altro vigile *"Per il Capo Turno"*.
- **Turno PROVA (solo admin):** nel menù turni trovi anche `PROVA (test)`. È una sandbox isolata — usa lo stesso turnario reale ma dati separati — quindi puoi fare tutte le prove che vuoi **senza toccare A/B/C/D**. I test sui turni veri, invece, **sì** che si riflettono sulle schermate di quel turno.
- **Cambio password admin:** da amministratore, **Impostazioni → Accesso → Cambia password**. Per i turni non c'è: il codice del distaccamento si cambia solo dalla Console (passo 3).
- **Dove si salvano i PDF:** su computer con **Chrome o Edge** si apre la finestra *Salva con nome*, dove scegli la cartella. Sul **telefono** si apre il menù *Condividi* (Salva su File, WhatsApp, Mail…). Su **Firefox/Safari da computer** il file va nei Download: per scegliere la cartella attiva nel browser l'opzione *"Chiedi sempre dove salvare i file"*.
- **Buoni pasto:**
  - **Personale del turno:** presente 1 · **rientro in straordinario (RS) 2 di giorno, 1 di notte** · sostituzione diurna (SD) 2 · notturna (SN) 1.
  - **Esterni in straordinario:** di giorno si sceglie **1 o 2 buoni** (parte da 2), di notte sempre 1.
  - **Esterni in sostituzione:** si sceglie **0, 1 o 2 buoni** di giorno, **0 o 1** di notte.
  - In **Impostazioni → Buoni pasto — regola del turno** ogni turno decide da cosa parte la scelta (straordinario diurno 1 o 2; sostituzione con o senza buono). I giorni già inseriti non cambiano.
  - Per correggere: in **Presenze giornaliere** tocca l'etichetta dell'esterno (*"Straordinario · 2 buoni"*) e scegli il numero giusto.
  - Nel PDF dei buoni: **ST** = straordinario, **SO** = sostituzione; il **2** accanto (ST2, SO2) vuol dire 2 buoni; giallo con buono, grigio senza.
- **Invia per email:** accanto a ogni "Genera PDF" c'è **Invia per email**. Il PDF si **salva sempre prima** (scegliendo la cartella), poi compare la finestra con **Apri l'email**: si apre il programma di posta con destinatario, oggetto e testo già scritti; si **trascina il PDF** nell'email e si preme Invia. Sul telefono il PDF viene allegato da solo dal menù Condividi.
  - Destinatari predefiniti: **presenze e ferie → fureria.siracusa@vigilfuoco.it**, **buoni pasto → informatica.siracusa@vigilfuoco.it**. Si cambiano in **Impostazioni → Invio per email** (anche più indirizzi, separati da virgola). Il turno PROVA non ha destinatari.
  - **Da fare una volta sul PC della caserma:**
    1. **Firefox → Impostazioni → Generale → Download → "Chiedi sempre dove salvare i file"**. Firefox non permette ai siti di aprire la finestra *Salva con nome*: senza questa opzione i PDF finiscono sempre nella cartella Download.
    2. Thunderbird deve essere il **programma di posta predefinito** di Windows (Impostazioni → App → App predefinite → Thunderbird). Se *Apri l'email* non apre nulla, si usa il pulsante **Copia** e si incolla in un'email nuova.
- **Backup:** in **Impostazioni → Backup → Esegui backup** salvi un file `.json` con tutto (personale, codici, mezzi, tutti i mesi di tutti gli anni). L'admin salva tutti i turni; dall'accesso del distaccamento si salva il turno aperto in quel momento. Consiglio: uno al mese, conservato in due posti (PC + chiavetta o cloud).
- **Ripristinare un backup (solo in emergenza):** Console Firebase → **Realtime Database** → Dati.
  - Backup dell'admin (*tutti-i-turni*): seleziona il nodo **`turni`** → menù ⋮ → **Importa JSON** → scegli il file.
  - Backup di un turno (*turnoD*…): apri `turni` → seleziona il nodo del turno (es. **`D`**) → ⋮ → **Importa JSON**.
  - Attenzione: l'importazione **sostituisce** quello che c'è in quel nodo con il contenuto del file.
- **Salto turno:** sopra i giorni del Prospetto mensile (e nei PDF ferie e buoni) c'è la riga **Salto turno**: una casella per ogni coppia diurno+notturno con il salto di quei due giorni (es. *Salto D2*). È calcolata dal turnario, per tutti i turni e tutti gli anni.
- **Tema:** di giorno è color carta, chiaro e riposante; di notte diventa scuro da solo se il telefono/PC è in modalità scura. Il pulsante tondo in alto a destra cambia tra *automatico → chiaro → scuro*, e ognuno se lo ritrova come l'ha lasciato.
- **Installarla sul telefono:** apri il sito, poi dal menù del browser scegli **"Aggiungi a schermata Home"** (Safari: tasto Condividi). Compare l'icona VVF e si apre a tutto schermo come un'app.
- **Connessione:** in alto a destra vedi **Online** (verde), **Salvataggio…** (giallo) oppure **Offline** (rosso). Se sei offline le modifiche restano in attesa: non chiudere la pagina finché non torna "Online" (se provi a chiuderla l'app ti avvisa).
