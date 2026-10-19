# PREVIO — Dashboard Offerte — riepilogo
Ultimo aggiornamento: 2026-10-19

## Cos'è e dove gira
- Percorso locale: `C:\Users\info\Documents\focusdata offerte\focusdata-dashboard\` (file unico `index.html` + `previo-logo.png`).
- Produzione: https://marioenatalia.github.io/focusdata-dashboard/ (GitHub Pages dal repo `marioenatalia/focusdata-dashboard`, branch `main`; il push pubblica da solo in 1-2 minuti).
- Stack: HTML + JavaScript senza build, Chart.js da CDN, logo studio e logo PREVIO incorporati in base64.
- Dati: tutti via API `https://focusdata-api.onrender.com` (repo `focusdata-api`, Node/Express su Render, DB PostgreSQL su Neon `focusdata_offerte`). La dashboard non ha un DB proprio.
- Architettura multi-studio: `ARCHITETTURA-MULTISTUDIO.md` nella cartella padre.

## Sezioni e funzioni
| Sezione / funzione | Cosa fa | File principali |
|---|---|---|
| Login (`#login-overlay`) | `doLogin()` → `POST /api/auth/login`, token in `localStorage.fd_token`; `caricaStudio()` + `applicaBranding()` per nome, colore e logo dello studio | index.html |
| Nuova offerta / anteprima | `renderPreview()` compone il documento: `testataDocumento()`, `bloccoProponente(data)`, tabelle servizi, `bloccoAccettazione(o)` (spazio firma o estremi firma OTP) | index.html |
| Offerte emesse | elenco, modifica, duplicazione, eliminazione (`loadOffers`, `deleteOffer`), stati firma OTP (`statiFirma`), pulsante «Rinvia OTP» | index.html |
| Segnalatori | scheda segnalatore con la stessa intestazione (`bloccoProponente`) | index.html |
| Operatori dello studio (`#operatori-area`) | gestione utenti del proprio studio (admin/superadmin) | index.html |
| Mandato SDD | testo del mandato di addebito (Qonto per FocusData) | index.html |

## Dati e dipendenze
- API: `/api/auth/*`, `/api/offerte`, `/api/segnalatori`, `/api/utenti`, `/api/studi` (superadmin), endpoint firma OTP.
- Nessuna variabile d'ambiente: l'URL dell'API è nel campo `api_url` (salvato in `localStorage.fd_api_url`).
- Versione gemella da tenere allineata: `C:\dashboardufficio\api\_offertaHtml.js` (Staff Dashboard) è il port di `renderPreview` e genera il PDF inviato in firma OTP.

## Punti delicati
- `bloccoProponente()` restituisce l'intestazione **fissa nel codice** quando lo studio è `focusdata` (o non è ancora caricato); per gli altri studi usa i dati della tabella `studi`. Ogni variazione dei dati societari di FocusData va fatta qui E in `_offertaHtml.js` dello Staff. Sede legale dal 16/09/2026: Via Sistina 121 – 00187 Roma (RI Roma prot. 407630/2026); unità locale Roncadelle (BS) invariata.
- I PDF già firmati non si rigenerano: restano com'erano al momento della firma (Venturelli n. 42 del 12/10/2026 ha ancora Via Ferruzzano 58).
- File unico di 2.700+ righe: modifiche minime, mai riformattazioni. Dopo il push controllare la pagina pubblicata (`curl` della pagina e `bloccoProponente()` dalla console).

## Registro modifiche
- 2026-10-19 — Sede legale FocusData nell'intestazione dei documenti: «Via Sistina 121 – 00187 Roma» al posto di «Via Ferruzzano 58 – 00173 Roma» (`bloccoProponente`) — file: index.html — commit 44c3951 — verificato: pagina GitHub Pages pubblicata con la stringa nuova, `bloccoProponente('19/10/2026')` dalla console della pagina online restituisce Via Sistina, nessun «Ferruzzano» nel documento; creato questo riepilogo
