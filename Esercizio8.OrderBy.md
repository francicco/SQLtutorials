# Tipi di ordinamento
In questa lettura, esplorerai l'uso della clausola ORDER BY per ordinare i dati. Hai appreso lo scopo della clausola ORDER BY e le diverse forme in cui può essere utilizzata per ordinare i dati. L'obiettivo principale di questa lettura è presentare ulteriori esempi relativi a scenari pratici dell'uso della clausola ORDER BY per ordinare i dati in una tabella.

## La clausola ORDER BY
La clausola ORDER BY è utile quando si desidera ordinare i risultati ottenuti durante l'esecuzione di una query SQL SELECT. I dati in un database diventano più significativi quando sono ordinati in un determinato modo. I dati ordinati aiutano le persone a prendere decisioni aziendali più accurate in modo efficace ed efficiente.

In SQL, c'è la clausola ORDER BY che può aiutarti a ottenere questo risultato. Se esegui una query SQL SELECT, ottieni un insieme di risultati non ordinati. Se desideri ordinarli, devi aggiungere la speciale clausola ORDER BY nella dichiarazione SQL SELECT.

Può essere utilizzata dopo la clausola FROM come segue:

```sql
SELECT *  
FROM Employee 
ORDER BY <nome colonna di ordinamento>; 
```

Dopo la parola chiave ORDER BY, è necessario specificare il nome della colonna in base ai dati che devono essere ordinati. Facoltativamente, puoi specificare le parole chiave ASC o DESC dopo il nome della colonna. Questo serve a indicare se l'ordinamento deve essere in ordine ascendente o discendente.

L'ordinamento in ordine ascendente e discendente sono i due principali tipi di ordinamento. Se ASC o DESC non sono specificati, i dati sono ordinati per impostazione predefinita in ordine ascendente. Le parole chiave ASC e DESC ordinano i dati in base alla colonna di ordinamento, tenendo conto del tipo di dati della colonna o del campo, ovvero intero, numerico, testo e date.

## Lavorare con la clausola ORDER BY
Esaminiamo alcuni scenari di esempio che utilizzano la clausola ORDER BY utilizzando le tabelle nel database di esempio. Puoi fornire dichiarazioni SQL SELECT utilizzando la clausola ORDER BY con le parole chiave ASC e DESC come richiesto per questi scenari.

Ordinamento per una singola colonna
Nella tabella dei clienti, i dati sono ordinati per impostazione predefinita in ordine ascendente all'interno del campo ID cliente. Il campo ID cliente è numerico, quindi i dati sono ordinati in ordine numerico ascendente. Ora esaminiamo come ordinare questi dati in ordine discendente del campo ID cliente.

Per fare ciò, puoi eseguire la seguente query:

```sql
SELECT * 
FROM customers 
ORDER BY CustomerId DESC; 
```

Nell'output, i record sono ordinati in ordine discendente (dal più grande al più piccolo) del CustomerId, che ha un tipo di dati numerico.

Ora esaminiamo come avviene l'ordinamento per una colonna di dati di tipo testo. Consideriamo di ordinare i dati per la colonna City, che ha un tipo di dati testo di VARCHAR. Se vuoi ordinare i dati del cliente per città, utilizza la seguente istruzione SELECT:

```sql
SELECT * 
FROM customers 
ORDER BY City; 
```

Un metodo di ordinamento come ASC o DESC non è stato specificato nella clausola ORDER BY. Quindi, per impostazione predefinita, l'ordinamento avviene in ordine ascendente.

Se esamini la colonna City, noterai che i dati sono ordinati in ordine alfabetico ascendente (da A a Z).

Ora eseguiamo la seguente istruzione SELECT:

```sql
SELECT * 
FROM customers 
ORDER BY City DESC; 
```

Ora noterai che i record sono ordinati in ordine alfabetico discendente (da Z ad A).

Esaminiamo un altro esempio di come i dati sono ordinati in una colonna di ordinamento che utilizza un campo di tipo DATE. Questo esempio utilizza la tabella delle fatture nel database di esempio. Puoi utilizzare la seguente istruzione SQL SELECT per ordinare i dati per la colonna della data della fattura:

```sql
SELECT * 
FROM invoices 
ORDER BY InvoiceDate;   
```

Se esamini la colonna InvoiceDate, noterai che i valori delle date sono ordinati da più piccoli a più grandi. Cioè, sono ordinati in ordine ascendente, che è l'ordine di ordinamento predefinito. Ora proviamo a eseguire questa query con la parola chiave DESC aggiunta nella clausola ORDER BY.

```sql
SELECT * 
FROM invoices 
ORDER BY InvoiceDate DESC; 
```

I dati sono ora ordinati dalla data più grande alla più piccola, che è l'ordine discendente.

## Ordinamento per più colonne
Puoi anche ordinare i dati per più colonne e applicare loro diversi ordini di ordinamento. Supponiamo che tu voglia ordinare i dati delle fatture sia per città di fatturazione che per data della fattura. Per fare ciò, es

egui la seguente query:

```sql
SELECT * 
FROM invoices 
ORDER BY BillingCity ASC, InvoiceDate DESC;    
```

Noterai che i dati sono ordinati in ordine ascendente per BillingCity. Ecco perché i dati nella colonna BillingCity sono ordinati in ordine alfabetico. I dati della colonna InvoiceDate sono a loro volta ordinati in ordine discendente.

Ad esempio, se esamini i record con la città di fatturazione di Amsterdam, le date della fattura sono ordinate in ordine discendente dalla data più grande alla più piccola. Allo stesso modo, se esaminassi da vicino gli altri insiemi di dati, osserveresti lo stesso.

I principali tipi di ordinamento in SQL sono ASC, ascendente, e DESC, discendente. Come i dati sono ordinati in questi due casi dipenderà dal tipo di dati del campo o della colonna utilizzato come colonna di ordinamento.
