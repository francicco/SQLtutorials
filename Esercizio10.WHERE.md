# La clausola WHERE
In questa lettura, esplorerai l'uso della clausola WHERE per filtrare i dati. Hai appreso lo scopo e la sintassi della clausola WHERE. Hai anche imparato come essa si comporta con diversi tipi di operandi (principalmente basati su testo o numerici) in base al tipo di dati della colonna della tabella. Hai esaminato i tipi di operatori che possono essere utilizzati nella clausola WHERE. L'obiettivo principale di questa lettura è presentare ulteriori esempi e scenari in cui la clausola WHERE viene utilizzata per filtrare i dati in una tabella.

## La clausola WHERE
La clausola WHERE è utile quando si desidera filtrare i dati in una tabella in base a una determinata condizione nello statement SQL. La clausola WHERE in SQL è presente allo scopo di filtrare i record e recuperare solo i record necessari. Questa può essere utilizzata nelle istruzioni SQL SELECT, UPDATE e DELETE.

Il filtraggio avviene in base a una condizione. La condizione può essere scritta utilizzando uno qualsiasi degli operatori di confronto o logici seguenti.

## Operatori di confronto
|Operatore             | Descrizione|
|=                              | Verifica se i valori di due operandi sono uguali o meno. Se sì, la condizione diventa vera.|
|!=                            | Verifica se i valori di due operandi sono uguali o meno. Se i valori non sono uguali, la condizione diventa vera.|
|<>                           | Verifica se i valori di due operandi sono uguali o meno. Se i valori non sono uguali, la condizione diventa vera.|
|>                             | Verifica se il valore del primo operando è maggiore del valore del secondo operando. Se sì, la condizione diventa vera.|
|<                             | Verifica se il valore del primo operando è minore del valore del secondo operando. Se sì, la condizione diventa vera.|
|>=                          | Verifica se il valore del primo operando è maggiore o uguale al valore del secondo operando. Se sì, la condizione diventa vera.|
|<=                          | Verifica se il valore del primo operando è minore o uguale al valore del secondo operando. Se sì, la condizione diventa vera.|
|!<                           | Verifica se il valore del primo operando non è minore del valore del secondo operando. Se sì, la condizione diventa vera.|
|!>                           | Verifica se il valore del primo operando non è maggiore del valore del secondo operando. Se sì, la condizione diventa vera.|

## Operatori logici
Operatore             | Descrizione
ALL                      | Utilizzato per confrontare un singolo valore con tutti i valori in un altro insieme di valori.
AND                   | Consente l'esistenza di condizioni multiple nella clausola WHERE di uno statement SQL.
ANY                   | Utilizzato per confrontare un valore con qualsiasi valore applicabile nell'elenco secondo la condizione.
BETWEEN           | Utilizzato per cercare valori che si trovano all'interno di un insieme di valori, dato il valore minimo e il valore massimo.
EXISTS                | Utilizzato per cercare la presenza di una riga in una tabella specificata che soddisfa un certo criterio.
IN                     | Utilizzato per confrontare un valore con un elenco di valori letterali specificati.
LIKE                  | Utilizzato per confrontare un valore con valori simili utilizzando operatori jolly.
NOT                   | Inverte il significato dell'operatore logico con cui è utilizzato. Ad esempio: NOT EXISTS, NOT BETWEEN, NOT IN, ecc. Questo è un operatore di negazione.
OR                    | Utilizzato per combinare condizioni multiple nella clausola WHERE di uno statement SQL.
IS NULL           | Utilizzato per confrontare un valore con un valore NULL.
UNIQUE             | Cerca ogni riga di una tabella specificata per l'unicità (nessun duplicato).

Utilizzando il database di esempio, esaminiamo un esempio che utilizza l'operatore di confronto > (maggiore di) per formulare la condizione della clausola WHERE per i criteri di filtraggio. Se desideri recuperare le fatture che hanno un valore totale superiore a $2, dovrai filtrare i record nella tabella delle fatture utilizzando la clausola WHERE nello statement SELECT.

Per eseguire questa azione, puoi eseguire la seguente query:

```sql
SELECT *
FROM invoices
WHERE Total > 2;
```

Noterai che questa query filtra i record in base alla condizione data nella clausola WHERE Total > 2. Include solo i record che hanno un valore totale superiore a $2. Ma cosa succede se desideri combinare più condizioni nella clausola WHERE? Le condizioni multiple nella clausola WHERE possono essere combinate utilizzando gli operatori logici AND / OR. Pertanto, questi due operatori sono anche noti come operatori congiuntivi.

La sintassi richiesta per utilizzare l'operatore AND nella clausola WHERE di uno statement SELECT è la seguente:

```sql
SELECT column1, column2, columnN
FROM table_name
WHERE [condition1] AND [condition2]...AND [conditionN];
```

N può essere qualsiasi numero. Qui, affinché l'intera condizione sia VERA, tutte le condizioni separate da AND devono essere VERE.

Esaminiamo un esempio. Hai bisogno di un elenco di fatture per le quali il totale supera $2 e il BillingCountry è gli USA. Ecco un esempio di come la condizione della clausola WHERE può essere data nello statement SELECT:

```sql
SELECT *
FROM invoices
WHERE Total > 2 AND BillingCountry = 'USA';
```

Qui, l'operatore AND viene utilizzato come operatore congiuntivo per combinare le due condizioni Total > 2 AND BillingCountry, che è gli USA. Riceverai i record delle fatture con un valore totale superiore a $2 con gli USA come paese di fatturazione. Ciò significa che affinché un record sia incluso nel risultato, entrambe le condizioni devono essere vere. Analogamente, l'operatore OR può anche essere utilizzato per combinare più condizioni nella clausola WHERE.

La sintassi è la seguente:

```sql
SELECT column1, column2, columnN
FROM table_name
WHERE [condition1] OR [condition2]...OR [conditionN];
```

Continuando a utilizzare la stessa tabella delle fatture per il prossimo esempio. Se desideri ottenere un elenco di fatture per le quali il Billing

Country è gli USA o la Francia, come useresti l'operatore OR per combinare le due condizioni?

Puoi scrivere la seguente sintassi SQL:

```sql
SELECT *
FROM invoices
WHERE BillingCountry = 'USA' OR BillingCountry='France';
```

Noterai che il risultato consiste in record in cui il paese di fatturazione è gli USA o la Francia. Ciò significa che affinché un record sia incluso nel risultato, una delle condizioni deve essere vera.

Consideriamo un altro scenario. Se desideri ottenere un elenco di fatture in cui il valore totale è superiore a $2 e il BillingCountry è USA o Francia, ecco la sintassi della query SELECT utilizzando entrambi gli operatori congiuntivi AND / OR insieme nella clausola WHERE:

```sql
SELECT *
FROM invoices
WHERE Total > 2 AND (BillingCountry = 'USA' OR BillingCountry = 'France');
```

Noterai che ha filtrato i record delle fatture che hanno un valore totale superiore a $2. Da quel risultato, ha anche filtrato i record che hanno un valore di paese sia gli USA sia la Francia. Nella query, le due condizioni combinate con l'operatore OR sono racchiuse tra parentesi per assicurarsi che siano valutate come un'unica espressione.

Gli altri operatori logici e di confronto SQL che non sono stati dimostrati in questa lettura possono anche essere utilizzati nella clausola WHERE. Inoltre, la clausola WHERE può essere utilizzata anche con le istruzioni UPDATE e DELETE. Per ulteriori informazioni, consulta la lettura delle risorse aggiuntive di questa lezione.
