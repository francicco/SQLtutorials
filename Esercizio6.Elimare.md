# Obiettivo

L'obiettivo di questo esercizio è insegnarti come eliminare i record. Avrai l'opportunità di praticare l'eliminazione di un record da una tabella in un database.

## Scenario

Il signor John Ericson possiede una piccola libreria. Il suo database della libreria include una tabella "Clienti" che contiene i dettagli dei clienti della libreria. L'immagine sotto mostra tutti i record di dati memorizzati nella tabella dei clienti.

Una schermata di tutti i record e dati memorizzati nella tabella dei clienti nel database della libreria.
Il signor Ericson desidera rimuovere il record del cliente Jimmy con numero ID 3.
I passaggi di seguito ti guideranno attraverso il processo per eliminare il record di Jimmy dal database della libreria.

## Istruzioni

Si presume che tu abbia già creato il database della libreria e la tabella dei clienti in un esercizio precedente.
Devi popolare la tabella dei clienti con dati rilevanti per completare questo esercizio.
Tuttavia, prima di copiare e incollare questo codice, assicurati di svuotare la tabella per evitare la duplicazione dei record.

Digita il seguente comando SQL:

```sql
TRUNCATE TABLE customers;
```

Quindi copia e incolla il seguente comando SQL nella sezione terminale SQL.

```sql
INSERT INTO 'customers' ('customerID', 'customerName', 'customerAddress') VALUES
(1, 'Jack', '115 Old street Belfast'),
(2, 'James', '24 Carlson Rd London'),
(4, 'Maria', '5 Fredrik Rd, Bedford'),
(5, 'Jade', '10 Copland Ave Portsmouth '),
(6, 'Yasmine', '15 Fredrik Rd, Bedford'),
(3, 'Jimmy', '110 Copland Ave Portsmouth');
```

Scrivi un comando SQL per eliminare il record di Jimmy dalla tabella dei clienti.

Seleziona il database per utilizzarlo digitando il seguente comando SQL.

```sql
USE bookshop;
```

Premi Invio.

Scrivi il comando SQL per eliminare il cliente.

1. Scrivi il comando "DELETE", seguito dalla parola chiave FROM per identificare la tabella di origine contenente i record. In questo caso, il nome della tabella è "customers".

2. Scrivi la clausola WHERE seguita dalla condizione per specificare il record che desideri eliminare. La condizione dovrebbe essere WHERE customerID = 3. La sintassi completa è la seguente:

```sql
DELETE FROM customers WHERE customerID = 3;
```

Esegui la query premendo il pulsante Invio sulla tastiera. La dichiarazione SQL viene eseguita e il record di Jimmy viene eliminato dal database della libreria.

Per visualizzare il contenuto della tabella dei clienti dopo l'eliminazione del record, puoi digitare la seguente dichiarazione di selezione:

```sql
SELECT * FROM customers;
```

Questa dichiarazione mostra tutti i dati esistenti nella tabella dei clienti.

Screenshot della dichiarazione di selezione che mostrerà tutti i dati esistenti nella tabella dei clienti.
Nota che il record del cliente con ID 3 è stato rimosso dal database della libreria.

In questo esercizio, hai praticato come eliminare un record di dati dal database usando il comando SQL DELETE.

_*Compito aggiuntivo (opzionale)*_ 

Il signor Ericson desidera eliminare il record di Yasmine. Il tuo compito ora è rimuovere i dettagli di questo cliente dalla tabella dei clienti.

Suggerimento: dovresti utilizzare il comando SQL "DELETE".
