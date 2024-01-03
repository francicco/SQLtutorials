# Clausa SELECT DISTINCT in uso

In questa lettura, esplorerai l'uso di SELECT DISTINCT per recuperare un insieme unico di valori in una dichiarazione SELECT. Hai appreso lo scopo e la sintassi di SELECT DISTINCT e come si comporta in una dichiarazione SELECT. L'obiettivo principale di questa lettura è presentare ulteriori esempi e scenari pratici che utilizzano la parola chiave DISTINCT nella dichiarazione SELECT.

## La parola chiave DISTINCT
DISTINCT è utile per recuperare un insieme di valori unici quando ci sono valori di colonna duplicati in una tabella. Viene utilizzato con la dichiarazione SELECT, quindi è comunemente chiamato SELECT DISTINCT. In breve, ciò che DISTINCT fa è trovare valori unici all'interno di una colonna o colonne di una tabella.

Esaminiamo alcuni esempi di come si comporta la parola chiave DISTINCT utilizzando alcuni scenari di recupero dati dalla tabella nel database di esempio.

Utilizzo di SELECT DISTINCT su una singola colonna
Se c'è una tabella chiamata invoices con lo stesso BillingCountry ripetuto in molte istanze, puoi eseguire la seguente query per identificarli:

```sql
SELECT BillingCountry
FROM invoices
ORDER BY BillingCountry;
```

Quando guardi il risultato, noterai che ci sono valori duplicati nella colonna BillingCountry. Come puoi ottenere un elenco di paesi di fatturazione unici in cui sono state emesse le fatture? Cambiamo la dichiarazione SELECT aggiungendo la parola chiave DISTINCT e poi eseguiamola nuovamente.

```sql
SELECT DISTINCT BillingCountry
FROM invoices
ORDER BY BillingCountry;
```

Questa volta, i valori duplicati sono scomparsi e viene restituito solo un insieme unico di paesi di fatturazione come risultato. Dove ci sono valori ripetuti nella colonna BillingCountry, ad esempio per l'Argentina, l'Australia e l'Austria. La query SELECT DISTINCT sopra eliminerà quelle righe duplicate e genererà il risultato come un insieme unico di valori.

## Utilizzo di SELECT DISTINCT su più colonne
Se ispezioni i valori nelle colonne BillingCountry e BillingCity, noterai che la stessa città di fatturazione si ripete per un singolo paese di fatturazione. Puoi eseguire il seguente codice per verificarlo.

```sql
SELECT BillingCountry, BillingCity
FROM invoices;
```

Quindi, come puoi generare un elenco di città di fatturazione uniche all'interno dei paesi di fatturazione?

Puoi eseguire una query che aggiunge la parola chiave DISTINCT alla dichiarazione SELECT.

```sql
SELECT DISTINCT BillingCountry, BillingCity
FROM invoices
ORDER BY BillingCountry, BillingCity;
```

Nota: La clausola ORDER BY è aggiunta qui per ordinare i valori per una facile consultazione.

Il risultato è un insieme unico di città di fatturazione recuperate per i paesi di fatturazione. Fondamentalmente, non ci sono valori duplicati nella colonna BillingCity. In altre parole, quando fai un DISTINCT di più colonne, cerca una combinazione di valori unici in tutte quelle colonne. In questo esempio, tutte le combinazioni di BillingCountry e BillingCity nel risultato sono uniche.

## Valori NULL in una colonna DISTINCT
Supponiamo che ci siano valori NULL in una colonna (o colonne) DISTINCT. Ad esempio, nella colonna BillingCity. Puoi eseguire la stessa query di prima per ottenere le città di fatturazione uniche nei paesi di fatturazione.

```sql
SELECT DISTINCT BillingCountry, BillingCity
FROM invoices
ORDER BY BillingCountry, BillingCity;
```

A condizione che per alcuni record la colonna BillingCity contenga valori NULL, riceverai record con una combinazione di qualche valore per BillingCountry e NULL per BillingCity.

Quindi, è importante sapere che SELECT DISTINCT tratta qualsiasi valore NULL nelle colonne DISTINCT come unico. Pertanto, in questo caso, cerca una combinazione di valori unici di BillingCountry e BillingCity. Qualsiasi valore NULL nella colonna BillingCity è considerato unico. Ad esempio, Argentina - NULL potrebbe essere una combinazione unica e Australia - NULL potrebbe essere un'altra.

## Utilizzo di DISTINCT con le funzioni aggregate SQL
DISTINCT può anche essere utilizzato con le funzioni aggregate SQL come COUNT, AVG, MAX e così via. In questo caso, è necessario specificare un'espressione scritta utilizzando qualche funzione aggregata. Quindi, non solo è possibile utilizzare DISTINCT con i nomi delle colonne, ma anche con le espressioni.

Cosa succede se vuoi sapere il numero di paesi unici dei clienti nella tabella dei clienti? Esegui una dichiarazione SELECT che utilizza la funzione aggregata COUNT sulla colonna del paese insieme a DISTINCT.

Ad esempio:

```sql
SELECT COUNT(DISTINCT country)
FROM customers;
```

Il risultato che ottieni è il numero di paesi unici dai quali provengono i clienti. Utilizzando DISTINCT sulla colonna/paese fornisce un elenco unico di paesi e la funzione aggregata COUNT conta il numero di risultati.

Ecco alcuni punti importanti da ricordare per quanto riguarda SELECT DISTINCT:

- Quando viene fornita solo una colonna o espressione nella clausola DISTINCT, la query restituirà i valori unici per quella colonna.
- Quando vengono fornite più colonne o espressioni nella clausola DISTINCT, la query recupererà combinazioni uniche per quelle colonne.
- La clausola DISTINCT non ignora i valori NULL nelle colonne DISTINCT. I valori NULL sono considerati come valori unici da DISTINCT.
