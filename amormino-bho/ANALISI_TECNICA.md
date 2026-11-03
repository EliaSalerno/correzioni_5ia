# Analisi Tecnica del Progetto — Sito_TPS

> Documento generato il 03/11/2026 a partire dal contenuto della cartella e dalla
> cronologia Git del repository.

---

## 1. Panoramica

| Voce | Valore |
|---|---|
| Nome | Sito_TPS |
| Tipo | Sito statico didattico (divulgazione circuiti digitali) |
| Stack | HTML5 + CSS3 (nessun JavaScript) |
| File sorgente | `index.html` (97 righe), `style.css` (178 righe) |
| Deployment | `Dockerfile` basato su `nginx:alpine` |
| Licenza | MIT |
| Autore | Alice Amormino (`alice.amormino@issgreppi.it`) |
| Periodo attività | 02/11/2026, 10:32 → 13:31 |
| Commit totali | 7 (inclusi merge e commit vuoti) |

### Contenuto del sito
- **Full Adder** (circuito combinatorio): descrizione + tabella della verità a 8 righe.
- **Latch SR** (circuito sequenziale): descrizione + tabella di funzionamento.
- Navigazione interna con ancore, layout a card, footer.

---

## 2. Struttura dei file

```
Sito_TPS/
├── index.html          # pagina unica, contenuto didattico
├── style.css           # foglio di stile unico, design system a variabili
├── Dockerfile          # immagine nginx:alpine + copia statica
├── README.md           # solo titolo (non documentato)
├── LICENSE             # MIT
├── .gitignore          # template Visual Studio (428 righe, generico)
├── log.md              # log Git (untracked)
└── logstat.md          # log Git con statistiche (untracked)
```

---

## 3. Analisi del codice

### 3.1 HTML (`index.html`)
**Punti di forza**
- Markup semantico corretto: `header`, `nav`, `main`, `section`, `footer`.
- `lang="it"`, `meta viewport` e `charset` presenti.
- Gerarchia dei titoli coerente (`h1` → `h2` → `h3`).
- Tabelle dati con `thead`/`tbody` e uso di `<sub>` per i pedici (`C_in`, `C_out`).
- Contenuto tecnico corretto: le tabelle di verità del Full Adder e del Latch SR
  sono entrambe esatte.

**Criticità**
- Le tabelle non hanno `caption` né `scope="col"` sulle intestazioni → accessibilità migliorabile.
- Il `nav` non ha `aria-label`.
- Nessun contenuto interattivo (i circuiti non sono simulati).
- README praticamente vuoto: assenza di istruzioni di avvio/deploy.

### 3.2 CSS (`style.css`)
**Punti di forza**
- Uso di **custom properties** (`:root`) come design token: buona manutenibilità.
- Palette coerente (rosa/magenta), ombre, gradienti e transizioni curate.
- `max-width` + `margin: auto` per il centraggio.
- `overflow-x: auto` sul wrapper delle tabelle → evita overflow orizzontale.
- `position: sticky` sulla navigazione.

**Criticità**
- **Nessuna media query**: layout non realmente responsive (padding `2.5rem`
  sulle card e font in `rem` fissi possono risultare scomodi su mobile).
- Font-size e spaziature non scalano con il viewport.
- Nessun supporto dark mode / `prefers-color-scheme`.
- Selettori globali (`p`, `h3`, `th`, `td`) non incapsulati: rischio di effetti
  collaterali se il progetto cresce.
- Mancano stati `:focus-visible` per la navigazione da tastiera.

### 3.3 Docker (`Dockerfile`)
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```
**Criticità**
- Manca un `.dockerignore`: vengono copiati anche `.git`, `log.md`, `logstat.md`,
  `README.md` e il Dockerfile stesso dentro l'immagine (immagine più pesante e
  potenziale esposizione di metadati del repository).
- `EXPOSE 80` è solo documentativo; manca una `CMD` esplicita (eredita quella di
  nginx, quindi funziona, ma è implicito).
- Non è presente una pipeline CI/CD né un `docker-compose.yml`.

### 3.4 Qualità generale
| Dimensione | Valutazione | Note |
|---|---|---|
| Correttezza contenuti | ⭐⭐⭐⭐⭐ | Tabelle di verità corrette |
| Semantica HTML | ⭐⭐⭐⭐ | Buona, accessibilità da rifinire |
| Architettura CSS | ⭐⭐⭐⭐ | Token e palette, manca responsive |
| Responsive design | ⭐⭐ | Nessuna media query |
| Documentazione | ⭐ | README minimo |
| DevOps | ⭐⭐ | Docker funzionante ma non ottimizzato |
| Test | ⭐ | Nessun test / validazione automatica |

---

## 4. Analisi della cronologia Git

### 4.1 Timeline dei commit

| # | Ora | Hash | Messaggio | Churn (righe) | % sul totale | % cumulata |
|---|---|---|---|---|---|---|
| 1 | 10:32:53 | `a8f946c` | Initial commit | 450 | 54,6% | 54,6% |
| 2 | 11:09:50 | `3e6660c` | prima commit | 28 | 3,4% | 58,0% |
| 3 | 11:16:39 | `4c3f275` | prima commit | 28 | 3,4% | 61,4% |
| 4 | 11:19:46 | `0291caf` | Merge branch 'main' | 0 | 0,0% | 61,4% |
| 5 | 11:22:49 | `9c932da` | Aggiunti file base | 0 | 0,0% | 61,4% |
| 6 | 11:28:32 | `ec487b6` | Aggiunta codice | 51 | 6,2% | 67,6% |
| 7 | 13:31:27 | `7e14e2f` | nuovo design | 267 | 32,4% | 100,0% |

> **Churn** = righe aggiunte + righe rimosse. Il commit #1 è quasi interamente
> boilerplate (`.gitignore` 428 righe, `LICENSE` 21, `README` 1): escludendolo,
> il codice realmente scritto è **374 righe**.
>
> Percentuali sul solo codice (374 righe): #2 = 7,5%, #3 = 7,5%,
> #6 = 13,6%, **#7 = 71,4%**.

### 4.2 Metriche temporali

| Metrica | Valore |
|---|---|
| Durata totale sessione | 2h 58m 34s (10:32:53 → 13:31:27) |
| Primo blocco attivo | 55m 39s (10:32:53 → 11:28:32) |
| Pausa tra blocco 1 e 2 | 2h 02m 55s (11:28:32 → 13:31:27) |
| Commit nel primo blocco | 6 |
| Commit nel secondo blocco | 1 (ma il più corposo: 267 righe) |
| Churn medio per commit (7) | ~118 righe |
| Churn medio per commit (escluso boilerplate) | ~62 righe |

### 4.3 Osservazioni sul flusso di lavoro
- **Commit duplicati/ribaltati**: `3e6660c` aggiunge `index.html` (+28), poi
  `4c3f275` lo elimina (−28), per poi essere reintrodotto in `ec487b6`.
  Indica confusione iniziale su branch/merge.
- **Commit vuoti**: `9c932da` crea `Dockerfile` e `style.css` vuoti.
- **Commit monolitico finale**: `7e14e2f` concentra il 32,4% del churn totale
  (71,4% del codice) in un'unica soluzione, con un salto qualitativo netto.
- **Nessun tag, nessun branch di feature, nessuna PR**: lavoro lineare su `main`.

---

## 5. Incidenza della collaborazione con l'IA (stima)

> **Nota metodologica**: non esiste alcuna prova diretta (nessun trailer
> `Co-authored-by`, nessun file di configurazione di assistenti). La seguente è
> una **stima indiziaria** basata su pattern del codice e della cronologia.

### 5.1 Indizi a favore di un intervento IA

| Indizio | Peso |
|---|---|
| Salto qualitativo netto tra i primi commit (28 righe HTML basilari) e il commit finale (design system completo) | Alto |
| Commit unico con **247 righe aggiunte / 20 rimosse** in ~2 ore di pausa | Alto |
| CSS con pattern tipici di generazione IA: design token in `:root`, palette coerente, gradienti, ombre multistrato, `transition`, `border-radius` armonici | Alto |
| Codice pulito e uniforme senza refactor intermedi (nessuna iterazione visibile) | Medio |
| Messaggi di commit generici ("nuovo design", "Aggiunta codice") | Medio |
| Struttura didattica molto ordinata e commenti in italiano essenziali | Basso |

### 5.2 Indizi a sfavore

| Indizio | Peso |
|---|---|
| Contenuto tecnico corretto e specifico (tabelle Full Adder / Latch SR) | Basso |
| Nessuna traccia esplicita di strumenti IA | — |
| Un singolo autore ha potuto scrivere tutto in autonomia | — |

### 5.3 Stima quantitativa

| Scenario | Incidenza IA stimata |
|---|---|
| Codice finale complessivo | **~55–70%** |
| Commit `7e14e2f` ("nuovo design") | **~70–85%** |
| Commit precedenti (`3e6660c`, `4c3f275`, `ec487b6`) | **~10–25%** |
| Probabilità complessiva di assistenza IA | **Alta (~70%)** |

**Interpretazione**: il progetto appare come un lavoro **iniziato manualmente**
(bozza HTML essenziale, creazione repo, Dockerfile) e poi **completato/rifinito
con l'ausilio di un'IA** nella fase di design. La parte più "IA-simile" è il CSS
finale (169 righe aggiunte), che presenta una maturità stilistica superiore a
quella dei commit precedenti. La logica dei contenuti resta invece coerente e
verificabile, quindi l'IA avrebbe agito soprattutto su **struttura e stile**,
non sulla correttezza tecnica.

---

## 6. Grafico orario di lavoro / percentuale di modifiche

Aprire il file **`grafico_orario_modifiche.html`** nel browser per il grafico
interattivo (SVG + CSS, nessuna dipendenza esterna).

### 6.1 Avanzamento cumulativo nel tempo (ASCII)

```
% cumulata
100 |                                                        ● 13:31 (267 righe)
    |                                                       /
 75 |                                                      /
    |                                                     /
 67 |                              ● 11:28 (51)
 61 |            ● 11:16 (28) ● 11:19 ● 11:22
 58 |      ● 11:09 (28)
 55 | ● 10:32 (450 boilerplate)
    +-------+-------+-------+-------+-------+-------+------→ ora
     10:30   11:00   11:30   12:00   12:30   13:00   13:30
```

### 6.2 Distribuzione del churn per commit

```
a8f946c 10:32  ████████████████████████████████████████████ 450  54,6%
3e6660c 11:09  ███ 28   3,4%
4c3f275 11:16  ███ 28   3,4%
0291caf 11:19  · 0     0,0%
9c932da 11:22  · 0     0,0%
ec487b6 11:28  █████ 51   6,2%
7e14e2f 13:31  ██████████████████████████ 267  32,4%
```

### 6.3 Lettura del grafico
- Il **54,6%** del churn è boilerplate iniziale, non lavoro di coding reale.
- Tra le 10:32 e le 11:28 si concentra il **13,0%** del churn totale
  (ma il **28,6%** del solo codice).
- Dopo una pausa di ~2 ore, il commit delle 13:31 scarica il **32,4%** del
  churn totale e il **71,4%** del codice: è il picco di produttività e,
  presumibilmente, il momento di maggior supporto IA.

---

## 7. Raccomandazioni

1. **Responsive**: aggiungere media query per tablet/mobile e ridurre i padding
   delle card sotto i 600px.
2. **Accessibilità**: `scope="col"`, `caption` sulle tabelle, `aria-label` sul
   `nav`, stati `:focus-visible`.
3. **Docker**: creare un `.dockerignore` (`.git`, `*.md`, `Dockerfile`) e
   aggiungere una `CMD` esplicita.
4. **Documentazione**: completare il `README.md` con descrizione, stack e
   istruzioni (`docker build` / `docker run`).
5. **Repository**: rimuovere i file di log (`log.md`, `logstat.md`) dal working
   tree o aggiungerli al `.gitignore`; uniformare la cronologia (commit vuoti e
   revert).
6. **Interattività (opzionale)**: aggiungere piccole simulazioni JS dei circuiti
   per aumentare il valore didattico.
7. **Trasparenza IA**: se è stata usata un'IA, dichiararlo nel README o nei
   messaggi di commit (`Co-authored-by`), utile in ambito scolastico.
