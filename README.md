# RAL → Netto — Calcolatore stipendio 2026

**Task tecnica — Application for Product Builder @ Jet HR (team Cost-Saving)**
Andrea Calzone

Prototipo che simula la proiezione di retribuzione netta annuale a partire da una RAL, mostrando ogni voce trattenuta al lordo — per il caso standard richiesto dal brief: impiegato a tempo indeterminato, residente a Milano, senza agevolazioni particolari.

**[→ Vedi il calcolatore live](https://andrecalzone97.github.io/jethr-calcolatore-ral-netto/)**

![Schermata principale del calcolatore](screenshot-hero.png)
![Dettaglio voce per voce delle trattenute](screenshot-dettaglio.png)

---

## Il problema, in una frase

Chi valuta un'offerta di lavoro in Italia guarda quasi sempre solo la RAL, ma quella non è la cifra che conta davvero — conta cosa resta in tasca. Questo tool prova a rispondere a una domanda semplice ("quanto mi resta al mese, davvero?") mostrando *come* ci si arriva, non solo il numero finale.

## Come ho lavorato

**1. Ricerca normativa.** Ho ricostruito la catena di calcolo dal lordo al netto a partire da fonti primarie e aggiornate al 2026: TUIR (art. 13, 15, 49, 51), Legge di Bilancio 2026 (L. 199/2025) per i nuovi scaglioni IRPEF, L. 207/2024 per il cuneo fiscale strutturale, delibere di Regione Lombardia e Comune di Milano per le addizionali, aliquote INPS correnti per i contributi dipendente. Ho incrociato più fonti (Agenzia delle Entrate, INPS, guide fiscali specializzate) per verificare che i numeri fossero coerenti tra loro, non presi da una singola pagina.

**2. Modellazione della logica.** Ho scomposto il calcolo nella sua sequenza reale (quella che userebbe un consulente del lavoro): contributi previdenziali dedotti *prima* del calcolo fiscale, IRPEF a scaglioni progressivi sull'imponibile risultante, detrazioni applicate all'imposta lorda (non al reddito), addizionali locali calcolate separatamente sull'imponibile. Ho fattorizzato la logica degli scaglioni progressivi (usata sia da IRPEF che da addizionale regionale) in un'unica funzione, per evitare di duplicare la stessa meccanica con numeri diversi.

**3. Scelte di semplificazione, esplicite.** Il dominio fiscale italiano è enorme; ho scelto consapevolmente cosa escludere per restare nel "caso semplice e standard" richiesto dal brief — carichi di famiglia, oneri detraibili, trattamento integrativo per redditi molto bassi, variazioni comunali diverse da Milano. Ogni esclusione è dichiarata nel tool stesso, non nascosta.

**4. Prototipo.** HTML/CSS/JS scritto a mano, un solo file, zero dipendenze esterne, zero tool no-code. Non perché fosse vietato usarli, ma perché la parte che conta in questa task è la logica di calcolo — un file leggibile dall'alto in basso è la prova più diretta di averla capita e tenuta sotto controllo.

## Cosa calcola

```
RAL
 → − Contributi INPS (9,19%, +1% oltre il massimale)
 → Imponibile fiscale
 → IRPEF lorda (scaglioni 23% / 33% / 43%)
 → − Detrazione lavoro dipendente (art. 13 c.1 TUIR)
 → − Ulteriore detrazione cuneo fiscale (L. 207/2024)
 → IRPEF netta
 → − Addizionale regionale Lombardia (scaglioni 1,23%–1,73%)
 → − Addizionale comunale Milano (0,80%, esente sotto 23.000€)
 → Netto annuo → ÷13 → Netto mensile
```

## Cosa non calcola (di proposito)

| Voce esclusa | Perché |
|---|---|
| Detrazioni per familiari a carico (art. 12 TUIR) | Il brief richiede il caso "senza agevolazioni particolari" |
| Oneri detraibili (spese mediche, mutuo, ecc.) | Si recuperano in dichiarazione, non mese per mese in busta paga |
| Trattamento integrativo (ex bonus Renzi) | Rilevante solo sotto i 15.000€ o in casi di incapienza specifica — fuori scope per una RAL 25-35k |
| Comuni diversi da Milano | Ogni comune ha una propria aliquota; Milano è l'ipotesi fissata dal brief |

Dettaglio completo delle semplificazioni, in linguaggio non tecnico, direttamente nel tool.

## Stack

HTML + CSS + JavaScript vanilla. Nessun framework, nessuna libreria, nessun tool no-code/AI-generativo per la UI. Un solo file, apribile in qualsiasi browser senza build step.

## Con più tempo, aggiungerei

- Selettore comune (aliquote comunali variabili su tutto il territorio italiano, non solo Milano)
- Detrazioni per carichi di famiglia (art. 12 TUIR)
- Simulazione part-time / apprendistato / tempo determinato
- Confronto RAL A vs RAL B side-by-side, utile per chi valuta più offerte

## Fonti principali

- TUIR — DPR 917/1986, artt. 12, 13, 15, 49, 51
- L. 199/2025 — Legge di Bilancio 2026 (scaglioni IRPEF)
- L. 207/2024 — cuneo fiscale strutturale
- Circolari INPS su aliquote contributive 2026
- Delibere Regione Lombardia e Comune di Milano su addizionali IRPEF

---

*Prototipo realizzato per la selezione tecnica Jet HR — non sostituisce un cedolino ufficiale o una consulenza fiscale.*
