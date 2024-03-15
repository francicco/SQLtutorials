# Obiettivo

L'obiettivo di questo esercizio è imparare l'utilizzo delle clausole SQL `ORDER BY` e `WHERE` per ordinare e filtrare i dati.

## Obiettivi

1. Utilizzare la clausola SQL `ORDER BY` per ordinare il risultato di una query.
2. Utilizzare la clausola SQL `WHERE` per specificare una condizione per il filtraggio dei record.

Esempio del database Chinook

In questo esercizio consideriamo il ben noto database di esempio "chinook", ampiamente utilizzato per dimostrazioni e scopi di test, lo userai per completare questo esercizio.

Il database di esempio Chinook è composto dal seguente schema di database.

### Prima di iniziare:

Per completare questo esercizio, devi eseguire i seguenti passaggi prima di configurare il database Chinook e creare la tabella Customer.

1. Crea il database Chinook e utilizzalo nel terminale MySQL.

```sql
CREATE DATABASE Chinook;
USE Chinook;
```

2. Crea la tabella dei clienti come segue (copia e incolla il codice nel terminale):

```sql
CREATE TABLE Customer (
    CustomerId INT NOT NULL,
    FirstName VARCHAR(40) NOT NULL,
    LastName VARCHAR(20) NOT NULL,
    Company VARCHAR(80),
    Address VARCHAR(70),
    City VARCHAR(40),
    State VARCHAR(40),
    Country VARCHAR(40),
    PostalCode VARCHAR(10),
    Phone VARCHAR(24),
    Fax VARCHAR(24),
    Email VARCHAR(60) NOT NULL,
    SupportRepId INT,
    CONSTRAINT PK_Customer PRIMARY KEY (CustomerId)
);
```

3. Inserisci i seguenti record di dati nella tabella dei clienti (copia e incolla il codice nel terminale):

```sql
INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (1, 'Luís', 'Gonçalves', 'Embraer - Empresa Brasileira de Aeronáutica S.A.', 'Av. Brigadeiro Faria Lima, 2170', 'São José dos Campos', 'SP', 'Brazil', '12227-000', '+55 (12) 3923-5555', '+55 (12) 3923-5566', 'luisg@embraer.com.br', 3);

INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (2, 'Eduardo', 'Martins', 'Woodstock Discos', 'Rua Dr. Falcão Filho, 155', 'São Paulo', 'SP', 'Brazil', '01007-010', '+55 (11) 3033-5446', '+55 (11) 3033-4564', 'eduardo@woodstock.com.br', 4);

INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (3, 'Alexandre', 'Rocha', 'Banco do Brasil S.A.', 'Av. Paulista, 2022', 'São Paulo', 'SP', 'Brazil', '01310-200', '+55 (11) 3055-3278', '+55 (11) 3055-8131', 'alero@uol.com.br', 5);

INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (4, 'Roberto', 'Almeida', 'Riotur', 'Praça Pio X, 119', 'Rio de Janeiro', 'RJ', 'Brazil', '20040-020', '+55 (21) 2271-7000', '+55 (21) 2271-7070', 'roberto.almeida@riotur.gov.br', 3);

INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (5, 'Mark', 'Philips', 'Telus', '8210 111 ST NW', 'Edmonton', 'AB', 'Canada', 'T6G 2C7', '+1 (780) 434-4554', '+1 (780) 434-5565', 'mphilips12@shaw.ca', 5);

INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (6, 'Jennifer', 'Peterson', 'Rogers Canada', '700 W Pender Street', 'Vancouver', 'BC

', 'Canada', 'V6C 1G8', '+1 (604) 688-2255', '+1 (604) 688-8756', 'jenniferp@rogers.ca', 3);
```

# Istruzioni

Prova a completare i seguenti compiti prima di continuare, in modo da poter verificare e confrontare le tue risposte con la nostra soluzione.

Compito 1: Scrivi una query SQL per visualizzare tutti i dati esistenti nella tabella dei clienti.

Compito 2: Scrivi una query SQL per ordinare il set di risultati in ordine ascendente per il nome.

Compito 3: Filtra i dati del set di risultati in base a una condizione in cui il paese è la Francia.

Compito 1: Visualizza i dati nella tabella dei clienti

Prima di iniziare a ordinare e filtrare i dati, visualizziamo alcuni dei dati dei clienti già presenti nel database. Ciò può essere fatto scrivendo la seguente query `SELECT`, che recupera tutti i dati dalla tabella dei clienti.

```sql
SELECT CustomerID, FirstName, LastName, City, State, Country FROM Customer;
```

Premi Invio per eseguire la query.

Visualizzazione dei dati dei clienti nella tabella a seguito dell'esecuzione corretta della query.

Ora vedrai molti dati sui clienti visualizzati sullo schermo, il che rende difficile trovare dati rilevanti su clienti specifici.

Compito 2: Ordina il set di risultati dei dati

Puoi semplificare la ricerca per gli utenti del database ordinando i dati. Ad esempio, puoi ordinare i dati in ordine alfabetico da A a Z utilizzando i nomi dei primi clienti. Ciò può essere fatto aggiungendo la clausola ORDER BY alla precedente query SQL come segue.

```sql
SELECT CustomerID, FirstName, LastName, City, State, Country
FROM Customer
ORDER BY FirstName;
```

Osserva ora che tutti i dati della tabella "customer" vengono visualizzati di nuovo. Tuttavia, i dati sono ora ordinati per la colonna "First Name" in ordine alfabetico da A a Z. Ciò rende più facile per gli utenti del database trovare i clienti che stanno cercando.

Compito 3: Filtra il set di risultati dei dati

Puoi rendere ancora più facile per gli utenti trovare clienti specifici filtrando i dati in base a determinati criteri. Ad esempio, puoi estrarre un elenco di clienti che provengono da un paese specifico.

In questo caso, puoi aggiungere la condizione alla precedente query SQL utilizzando la clausola "WHERE" come mostrato di seguito e premere Invio per eseguire la query.

```sql
SELECT *
FROM Customer
WHERE Country = "Canada";
```

Il risultato della query SQL mostra tutti i clienti solo dal Canada.

Per renderlo ancora migliore, puoi visualizzare tutti i clienti in Canada con l'ordine alfabetico da A a Z!

Per farlo, puoi aggiungere la clausola ORDER BY alla fine della precedente query SQL come segue:

```sql
SELECT *
FROM Customer
WHERE Country = "Canada"
ORDER BY FirstName;
```

Il risultato della query SQL mostra ora tutti i clienti dal Canada con l'ordine alfabetico corretto.

Hai ora imparato come ordinare e filtrare i dati utilizzando le clausole SQL ORDER BY e WHERE.

# Esercizio aggiuntivo (opzionale)

Devi scrivere una query SQL per visualizzare solo il nome e il paese dei clienti provenienti dal Canada.

Una volta completata la query SQL, esegui la query.

Soluzione

```sql
SELECT FirstName, Country
FROM Customer
WHERE Country = "Canada"
ORDER BY FirstName;
```

```
+-----------+---------+
| FirstName | Country |
+-----------+---------+
| Jennifer  | Canada  |
| Mark      | Canada  |
+-----------+---------+
```
