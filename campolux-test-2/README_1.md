# Sito Campolux (statico)

Sito di una pagina, senza framework e senza build: solo HTML + CSS.
Nessun cookie, nessun tracciamento, nessun servizio esterno → non serve il banner cookie.

## Struttura

```
index.html     la pagina
style.css      lo stile
favicon.svg    icona
img/           (da creare) qui vanno le foto
```

## Da completare prima di pubblicare

Nel sito ho usato solo dati trovati in rete (storia dal 1961, indirizzo, telefono 010 921028). Mancano:

- [ ] **Orari di apertura**: in `index.html`, in fondo, nello `<script>`, la costante `HOURS` (c'è un esempio da copiare). Finché è `null` il sito mostra "Orari in aggiornamento"
- [ ] **Email per le prenotazioni**: nello stesso `<script>`, la costante `BOOKING_EMAIL`. Finché è vuota il modulo "Prenota una visita" invita a chiamare al telefono
- [ ] **Email di contatto** da mostrare nella sezione "Dove siamo" (c'è un commento TODO)
- [ ] **P.IVA / C.F. / REA** nel footer (obbligatori per i siti di attività commerciali in Italia)
- [ ] **Foto** del negozio e dei lavori (lampadari esposti, ristrutturazioni, chiese). La galleria è già pronta ma spenta: vedi "Aggiungere le foto" più sotto
- [ ] Controllare che il **segnaposto sulla mappa** cada sul negozio (le coordinate vengono dai dati pubblici dell'azienda)
- [ ] **Controllo dei testi** con i titolari: ho riscritto con parole mie quello che era sul vecchio sito, vanno verificati
- [ ] **Marchi trattati**: ora nella sezione "I marchi che trattiamo" c'è solo Artemide. Servono gli altri, con il sito ufficiale di ciascuno (vedi "Marchi" più sotto)
- [ ] Un link a Facebook/Instagram, se esistono
- [ ] Controllare i **testi della storia** ("All'inizio sono tra i primi in Liguria a fabbricare lampadari…"): la produzione è raccontata solo come storia dell'azienda, non come attività di oggi. Se i titolari preferiscono non parlarne affatto, si cancella la frase

## Funzioni del sito

**Interruttore giorno/notte** (in alto a destra). Toccandolo il tema cambia con una transizione a cerchio che parte dall'interruttore: di notte il lampadario si accende, di giorno è spento. La scelta viene ricordata. Alla prima visita il sito segue la preferenza del dispositivo (chiaro/scuro). Per aprire sempre in modalità notte, vedi il commento nello `<script>` dentro `<head>` di `index.html`. Chi ha attivato "riduci animazioni" vede il cambio senza movimento.

**Orari di apertura.** Si scrivono una volta sola in `HOURS`. Il sito mostra la tabella, evidenzia il giorno di oggi e scrive "Aperto ora, fino alle…" oppure "Chiuso ora, riapriamo…" (ora italiana). Nel modulo di prenotazione i giorni di chiusura vengono rifiutati.

**Prenota una visita.** Il modulo non usa nessun server: quando il visitatore preme "Invia la richiesta" si apre il suo programma di posta con una mail già compilata, indirizzata a `BOOKING_EMAIL`, e lui deve premere "Invia". È semplice e non raccoglie dati sul sito, ma chi chiude la finestra della posta non invia nulla. Se in futuro vorrete un invio vero senza passare dalla posta del visitatore, si può collegare un servizio gratuito come Formspree o Web3Forms.

**Mappa.** Sotto "Dove siamo" c'è una mappa di OpenStreetMap che si carica solo quando il visitatore preme "Mostra la mappa": così prima del clic nessun dato va a servizi esterni e non serve il banner cookie. Di notte la mappa si scurisce insieme al sito. Per spostare il segnaposto cambia le coordinate nello `<script>`, nella riga con `openstreetmap.org/export/embed.html` (`marker=latitudine,longitudine`) e nei link "Apri in…" nella stessa sezione.

## Marchi

Nella sezione "I marchi che trattiamo" ogni marchio è un nome scritto in testo che, cliccato, apre il sito ufficiale dell'azienda in una nuova scheda. Per aggiungerne uno, in `index.html` copia la riga `<li>` di Artemide (c'è un modello già pronto nel commento sotto) e cambia indirizzo e nome. Controlla sempre che il link si apra.

Ho usato il nome in testo e non i loghi di proposito: i loghi sono marchi registrati e ogni azienda ha le sue regole d'uso. Se un'azienda mette a disposizione i suoi loghi ai rivenditori (spesso in un'area riservata o su richiesta), si possono sostituire ai nomi.

## Aggiungere le foto

1. **Prepara le foto.** Una per volta, lato lungo circa 1600 px, formato JPG o WebP, meglio sotto i 300 KB ciascuna (su https://squoosh.app si ridimensionano e si comprimono dal browser, senza installare nulla). Foto da telefono da 5 MB rallenterebbero molto il sito, soprattutto da cellulare.
2. **Dai nomi semplici**, senza spazi né accenti: `negozio-1.jpg`, `lavoro-chiesa.jpg`, ecc.
3. **Mettile in una cartella `img`** accanto a `index.html`.
4. In `index.html` cerca il commento "GALLERIA FOTO". Cancella la riga con `<!-- GALLERIA-INIZIO` e quella con `GALLERIA-FINE -->`: la sezione "Il negozio e i nostri lavori" compare.
5. Per ogni foto c'è una riga `<figure>`: cambia il nome del file (due volte, nel link e nell'immagine), **`alt`** (cosa si vede, per chi non vede l'immagine e per Google) e la didascalia, che puoi svuotare. Per aggiungere una foto copia una riga, per toglierla cancellala.
6. Se un gruppo non ha foto (per esempio non avete ancora quelle del negozio), cancella per intero il suo `<div class="gallery-group">`.

Cliccando una foto si apre ingrandita, con frecce (o tasti ← →), chiusura con la X, con Esc o toccando lo sfondo. Le sezioni della pagina alternano da sole lo sfondo, quindi non serve toccare altro.

Attenzione a **chi compare nelle foto**: se si riconoscono clienti o dipendenti, serve il loro consenso a pubblicarle.

## Provare il sito in locale

Basta aprire `index.html` con un doppio clic. In alternativa, con un mini server:

```
python3 -m http.server 8000
```
poi vai su http://localhost:8000

## Pubblicazione (deploy)

Prima cosa da chiarire con i titolari: **chi possiede il dominio `campolux.it`** (quale registrar: Aruba, Register, GoDaddy, Register.it...) e **dove puntano ora i DNS**. Il vecchio gestore potrebbe avere ancora le credenziali: serve il codice di accesso al pannello, oppure farsi intestare/delegare il dominio.

### Opzione A – Cloudflare Pages (consigliata, gratuita)

1. Crea un account su https://dash.cloudflare.com
2. *Workers & Pages* → *Create* → *Pages* → **Upload assets**
3. Trascina la cartella del sito (quella con `index.html`) e premi *Deploy*
4. Ottieni subito un indirizzo `nome.pages.dev` per fare le prove
5. *Custom domains* → aggiungi `campolux.it` e `www.campolux.it` e segui le istruzioni DNS
6. Il certificato HTTPS è automatico

### Opzione B – Netlify (gratuita, ancora più semplice)

1. Vai su https://app.netlify.com/drop
2. Trascina la cartella del sito nella pagina: è online in pochi secondi
3. *Domain management* → *Add a domain* → `campolux.it`
4. HTTPS automatico

### Opzione C – GitHub Pages (gratuita, utile per tenere lo storico delle modifiche)

1. Crea un repository su GitHub e carica i file
2. *Settings* → *Pages* → Source: branch `main`, cartella `/ (root)`
3. In *Custom domain* inserisci `campolux.it`

### Opzione D – Hosting tradizionale (se il dominio ha già un hosting con FTP)

Carica `index.html`, `style.css`, `favicon.svg` (e `img/`) nella cartella `public_html` (o `www`) via FTP, ad esempio con FileZilla. Verifica che il certificato HTTPS sia attivo dal pannello.

### Collegare il dominio (DNS)

Dal pannello del registrar, in base alla scelta:

| Servizio | Cosa configurare |
|---|---|
| Cloudflare Pages | Più semplice spostare i nameserver su Cloudflare (te li indica lui), oppure `CNAME www` → `nome.pages.dev` |
| Netlify | `A @` → indirizzo indicato da Netlify, `CNAME www` → `nome.netlify.app` |
| GitHub Pages | 4 record `A @` verso gli IP indicati dalla documentazione GitHub, `CNAME www` → `utente.github.io` |

I valori esatti cambiano nel tempo: usa quelli che mostra il servizio nella schermata del dominio. La propagazione DNS può richiedere da pochi minuti a 24 ore.

### Se il vecchio sito è ancora online

Prima di cambiare i DNS controlla cosa risponde oggi `campolux.it` e, se ci sono caselle email sul dominio (`@campolux.it`), **non toccare i record MX**: spostare i DNS senza copiarli farebbe smettere di funzionare la posta.

## Modificare il sito in futuro

- Testi: apri `index.html` con un editor (VS Code, Blocco Note) e cambia il testo tra i tag
- Colori e font: variabili all'inizio di `style.css` (`:root`)
- Nuove foto: mettile in `img/`, poi attiva la galleria in `index.html`
- Dopo ogni modifica, ricarica la cartella su Cloudflare/Netlify (o fai il commit su GitHub)

## Font

Per evitare caricamenti da Google (privacy/GDPR) il sito usa solo font di sistema. Se vorrai un font personalizzato, scaricalo e servilo dalla stessa cartella con `@font-face`.
