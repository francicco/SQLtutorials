# Obiettivo

Lo scopo di questo esercizio è guidarti attraverso un processo passo-passo per la creazione di un database, la creazione di una tabella nel database e l'inserimento di dati nella tabella. L'obiettivo è offrirti l'opportunità di praticare come creare un database, una tabella e inserire dati.

## Scenario

Il signor John Ericson è proprietario di una piccola libreria. Decide di creare un database digitale per gestire i dati dei suoi clienti elettronicamente anziché utilizzare carta e penna. In questo esercizio, costruirai il database della libreria.

### Istruzioni

Prova a svolgere i compiti seguenti prima di continuare, in modo da poter controllare e confrontare le tue risposte con la nostra soluzione.

**Compito 1: Creare un database chiamato bookshop.**

Dentro il terminale, scrivi il comando "CREATE DATABASE" seguito dal nome del tuo nuovo database. In questo caso, il nome del database si chiama "bookshop". Infine, aggiungi un punto e virgola alla fine della dichiarazione e premi Invio sulla tastiera per eseguire la query.

```sql
CREATE DATABASE bookshop;
```

**Compito 2: Creare una tabella chiamata customers con ID cliente, nome e indirizzo.**

1. Per creare la tabella customers all'interno del database bookshop, devi prima selezionare il database bookshop digitando:

```sql
USE bookshop;
```

2. Scrivi la dichiarazione create table.

Scrivi un'istruzione SQL che contenga il comando CREATE TABLE seguito dal nome della tabella, che è "customers" in questo caso. Aggiungi quindi una parentesi aperta per definire le colonne della tabella, tra cui l'ID cliente, il nome del cliente e l'indirizzo del cliente.

Ovviamente, ogni colonna deve essere assegnata a un tipo di dati adatto. Una volta definite tutte le colonne necessarie, devi aggiungere una parentesi chiusa e un punto e virgola alla fine dell'istruzione SQL come segue:

```sql
CREATE TABLE customers (customerID int, customerName varchar(50), customerAddress varchar(255));
```

3. Premi Invio per eseguire l'istruzione SQL. Il risultato di output sottostante conferma che la query è OK, il che significa che la tabella è stata creata con successo.

4. Per mostrare la tabella che hai già creato, puoi semplicemente digitare:

```sql
SHOW tables;
```

**Compito 3: Inserire un record di dati per un cliente.**

1. Ora, inseriamo i dati del nostro primo cliente nella tabella dei clienti! Ciò può essere fatto scrivendo la seguente istruzione SQL, che inserisce un insieme di valori pertinenti che corrispondono ciascuno a una colonna specifica.

```sql
INSERT INTO customers (customerID, customerName, customerAddress) VALUES (1, "Jack", "115 Old Street Belfast");
```

In questa istruzione, dichiari la clausola INSERT INTO, seguita dal nome della tabella. In questo caso, è la tabella "customers". Quindi aggiungi i nomi delle colonne all'interno di una coppia di parentesi. Successivamente, inserisci la parola chiave VALUES e quindi aggiungi i valori che desideri assegnare a ogni colonna all'interno di una coppia di parentesi. In questo caso, l'ID è 1, il nome è "Jack" e l'indirizzo è "115 Old Street, Belfast". Nota che tutti i valori delle colonne VARCHAR devono essere scritti tra virgolette doppie, mentre i numeri non richiedono questo.

2. Premi Invio per eseguire l'istruzione SQL.

Il risultato di output sottostante conferma che la query funziona, il che significa che il record dei dati è stato inserito con successo.

3. Puoi digitare la seguente istruzione SQL select per ottenere il contenuto della tabella dei clienti dopo l'inserimento dei dati.

```sql
SELECT * FROM customers;
```

4. Premi Invio. Il risultato di output sono tutti i record di dati che esistono nella tabella dei clienti, come mostrato di seguito:

In questo esercizio, hai praticato come creare una tabella nel database e come inserire dati nella tabella. Ecco un compito aggiuntivo facoltativo per testare le tue abilità. 

**Compito aggiuntivo (facoltativo)**

Il signor Ericson desidera inserire un altro record di dati per un altro cliente, con i seguenti dettagli: l'ID è 2, il nome è "James" e l'indirizzo è "24 Carlson Road, London". Ora il tuo compito è aggiungere i dettagli del cliente nella tabella dei clienti.

**Soluzione:**

Scrivi la seguente istruzione SQL nell'editor SQL nel tuo terminale, quindi premi il pulsante Go per eseguirlo.

```sql
INSERT INTO customers(customerID, customerName, customerAddress) VALUES (2, "James", "24 Carlson Rd London");
```
