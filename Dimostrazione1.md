# SQL: Preparare un Database Step by Step.
## Apri il dataset.
Questo dataset proviene dal set di dati associato all'ispezione sanitaria delle navi da crociera. Le variabili incluse sono:

1. *Ship Name*: identifica la nave da crociera.
2. *Month-Year of Inspection*: indica il mese e l'anno in cui è stata effettuata l'ispezione.
3. *Risk Classification*: la classificazione del rischio assegnata alla nave, con quattro possibili livelli (A, B, C, D), dove A rappresenta condizioni sanitarie eccellenti con un punteggio di rischio fino a 150, e D indica condizioni sanitarie insoddisfacenti con rischio punteggio superiore a 450.
4. *Risk Score*: un punteggio che riflette il rischio associato in base all'ispezione sanitaria. Valori più alti indicano un rischio maggiore.
5. *Compliance Index*: rappresenta la percentuale di elementi di ispezione che sono stati rispettati dalla nave.
6. *Season*: si riferisce alla stagione del Programma nazionale di sorveglianza sanitaria per le navi da crociera, che generalmente inizia ad ottobre e continua fino ad aprile dell'anno successivo.

Si segnala inoltre che esiste una regola per il declassamento della classificazione: se durante un'ispezione non viene implementato un controllo critico, la nave viene declassata al successivo standard di rischio. Il punteggio di rischio (PSR) viene calcolato sommando i valori di rischio di ciascun elemento dalla checklist di ispezione. L’assenza di controlli o le carenze nei controlli aumentano il punteggio di rischio, indicando una minore conformità e un rischio sanitario più elevato.

Ora ci troviamo di fronte a un problema aziendale. Analizzeremo il programma ANVISA per le ispezioni sanitarie sulle navi da crociera, avendo a nostra disposizione i dati e il dizionario dei dati. Successivamente, prepareremo il database, successivamente lo caricheremo e inizieremo la nostra analisi.

## Iniziamo
Procediamo con MySQL Workbench. Approfondiremo direttamente un concetto di fondamentale importanza: gli Schemi. I sistemi di gestione dei database (DBMS) utilizzano il concetto di schema.

A seconda del DBMS, l'idea può essere essenzialmente la stessa, sebbene sia possibile avere più schemi all'interno dello stesso database. Per i nostri scopi, il punto chiave da comprendere è il seguente.

![1_4IJBtrPjGsK1w3cd0ulOxg](https://github.com/francicco/SQLtutorials/assets/9006870/a5cc3bd5-1b54-4933-ba62-11d18a3cd711)

Lo schema "sys" è inerente al DBMS SQL e non è consigliato per operazioni definite dall'utente. La manomissione di questo schema potrebbe compromettere l'integrità dell'installazione DBMS. Inizieremo invece un nuovo schema.

È sufficiente fare clic con il pulsante destro del mouse sullo spazio vuoto della scheda schemi e selezionare Crea schema. Il nostro nuovo schema si chiamerà cap02, in linea con il secondo capitolo della nostra serie SQL from Scratch to Data Science.

Creeremo uno schema per ogni capitolo secondo necessità. Possiamo lasciare le opzioni Set di caratteri e Collazione impostate su Predefinito. Queste impostazioni determinano essenzialmente il formato dei caratteri archiviati nel database, ma le impostazioni Default Charset e Default Collation sono sufficienti per i nostri requisiti.

```sql
CREATE SCHEMA `cap02`
```

Si noti che ci presenta un'esatta istruzione SQL, un'istruzione DDL (Data Definition Language), progettata per creare un oggetto nel database. In questo caso, CREATE SCHEMA, seguito dal clic su Applica, creerà correttamente lo schema.

Osserva che ora appare lo schema "cap02" che abbiamo creato e nota anche la presenza di una sorta di registro in basso che dettaglia tutte le azioni eseguite. È sempre utile monitorare l'output dell'azione per verificare eventuali messaggi di errore.
