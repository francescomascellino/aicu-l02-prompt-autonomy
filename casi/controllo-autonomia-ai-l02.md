# Controllo Autonomia AI - L02

## Rubrica Da Usare

Per ogni caso considera:

- chiarezza del task;
- sensibilita' di dati o codice;
- blast radius della richiesta;
- reversibilita' della modifica;
- costo di verifica;
- permesso massimo da concedere;
- strategia del prompt: zero-shot o few-shot;
- evidenze richieste all'AI: assunzioni, criterio e verifica, non chain-of-thought estesa.

## Caso 1

### Richiesta

```txt
Non so dove viene renderizzata la lista dei ticket.
```

### Modalita' Scelte

Plan

### Rischio

Basso

Criterio principale:

```txt
Accesso di sola lettura ai files della repo
```

Motivazione:

```txt
Non conosciamo la posizione di un componente all'interno della repository, ulteriori permessi oltre a quelli di analisi non sono necessari.
```

### Prompt Operativo

Strategia prompt: zero-shot

Evidenze richieste:

- Assunzioni: Siamo a conoscenza della presenza del componente, possiamo visualizzarne la presenza a schermo, non siamo a conoscenza dello scaffolding e della struttura del progetto.
- Criterio: Plan: necessitiamo di comprendere la posizione del componente e le sue eventuali dipendenze.
- Verifica: Manuale

```txt
## Obiettivo
Trovare il componente che renderizza la lista dei ticket nel progetto.

## Contesto
- Non conosco la struttura del progetto
- Non so quale framework viene usato

## Cosa fare
- Indica i pattern di ricerca più adatti per localizzare il componente
- Proponi una strategia basata su: nome file, import, route, o componente padre
- Specifica quali informazioni servono per restringere la ricerca

## Output atteso
- Una lista di passi per trovare il file
- Eventuali domande da porre per restringere la ricerca
```

### Prova Di Controllo

```txt
I risultati trovati dal modello risultano essere quelli richiesti
```

## Caso 2

### Richiesta

```txt
Migliora tutta la UX della dashboard ticket, aggiungi filtri, rendila piu' moderna e sistema eventuali problemi.
```

### Modalita' Scelte

Fermarsi/Chat

### Rischio

Alto

Criterio principale: 

```txt
Nessun accesso a file della repo
```

Motivazione:

```txt
Abbiamo chiesto un compito non atomico con blast-off ampio
```

### Prompt Operativo

Strategia prompt: zero-shot

Evidenze richieste:

- Assunzioni: Siamo arrivati in un momento critico in cui sono necessarie molte modifiche che non possiamo eseguire con una unica query.
- Criterio: Cercare di ridurre la query in parti atomiche e lavorare su di esse.
- Verifica: Otteniamo un piano di azione dalla quale elaborare prompt atomici

```txt
Hai questo task: "Migliora tutta la UX della dashboard ticket, aggiungi filtri, rendila più moderna e sistema eventuali problemi."

Il task è troppo ampio e rischioso. Genera un piano per dividerlo in task atomici a basso rischio.

## Cosa produrre
Per ogni task:
- Nome breve
- Obiettivo specifico
- Cosa fare (max 3 punti)
- Cosa NON fare (1 punto)
- Come verificare

## Regole
- Il primo task deve essere solo osservazione/analisi (zero modifiche)
- Ogni task deve essere indipendente (commit separata)
- Ordina i task per dipendenza
- Massimo 6 task
- Non produrre codice, solo il piano

## Output atteso
Una lista numerata di task con i campi sopra.
```

### Prova Di Controllo

```txt
Otteniamo una lista di task minimi con obiettivi e verifiche da poter espandere successivamente in base alle necessità nascenti.
```
Esempio di punto 1 del Plan proposto dal Modello:
```txt
1. Diagnosi Dashboard
Obiettivo: Comprendere lo stato attuale senza modificare nulla.

Cosa fare:

Individuare il componente principale della dashboard
Elencare i problemi UX evidenti
Documentare lo stile attuale (colori, font, spaziatura)
Cosa NON fare: Modificare qualsiasi file.

Come verificare: Output è un documento di analisi. Zero commit.
```
## Caso 3

### Richiesta

```txt
Dobbiamo aggiungere un messaggio quando non ci sono ticket aperti, ma prima voglio capire quali file toccherebbe.
```

### Modalita' Scelte

Plan

### Rischio

Basso

Criterio principale:

```txt
Accesso di sola lettura ai files della repo
```

Motivazione:

```txt
Non conosciamo la posizione delle dipendenze all'interno della repository, ulteriori permessi oltre a quelli di analisi non sono necessari
```

### Prompt Operativo

Strategia prompt: zero-shot

Evidenze richieste:

- Assunzioni: Siamo a conoscenza della posizione del componente ma non delle sue dipendenze.
- Criterio: necessiamo di comprendere la posizione delle dipendenze.
- Verifica: Manuale

```txt
Conosco il componente che mostra la lista dei ticket, ma non le sue dipendenze.
Prima di intervenire, analizza quali file e servizi verrebbero coinvolti nell'aggiungere un messaggio di empty state.

## Contesto
- Il componente è noto (indicalo)
- Le sue dipendenze (hook, context, servizi) non sono note
- Il progetto è sconosciuto

## Cosa produrre
1. Piano di analisi: strategia per mappare le dipendenze
2. Lista dei file che verrebbero toccati, con motivazione per ognuno
3. Domande da porre per restringere l'analisi

## Vincoli
- Non modificare nulla
- Non produrre codice
- Output: piano + lista file
```

### Prova Di Controllo

```txt
Vengono elencate le dipendenze del componente richiesto
```

## Caso 4

### Richiesta

```txt
Dopo aver approvato il piano, applica la modifica minima per mostrare il messaggio nello stato senza ticket.
```

### Modalita' Scelte

Modificare

### Rischio

Medio

Criterio principale:

```txt
Accesso al componente
```

Motivazione:

```txt
Necessitiamo di applicare una modifica pianificata
```

### Prompt Operativo

Strategia prompt: zero-shot / few-shot

Evidenze richieste:

- Assunzioni: Sappiamo la struttura. Sappiamo dove il componente andrà modificato
- Criterio:
- Verifica: Controllo manuale delle funzionalità e delle modifiche

```txt
Dopo aver approvato il piano, applica la modifica minima per mostrare il messaggio nello stato senza ticket.

## Cosa fare
- Individuare la condizione che determina "lista vuota"
- Aggiungere il messaggio inline nel componente esistente visibile solo quando la condizione lista vuota è vera.
- Messaggio: testo fisso, max 2 righe, con emoji rassicurante

## Cosa NON fare
- Creare nuovi file o componenti
- Aggiungere dipendenze
- Modificare stile o layout

## Verifica
- Messaggio appare solo quando la lista è vuota
- Zero errori in console
- Layout invariato
```

### Prova Di Controllo

```txt
Messaggio appare solo quando la lista è vuota
```

## Nota Finale

Caso 2. Se utilizzato senza criterio può portare a una totale riscrittura del progetto, perdita di dati esistenti, modifiche non tracciabili.
