# Obiettivo

Praticare come scegliere tipi di dati adatti per diversi tipi di colonne nella tabella.

## Obiettivi dell'esercizio

Questo esercizio dimostra come scegliere tipi di dati adatti per una varietà di colonne per memorizzare dati come stringhe, interi, date e valori decimali.

## Scenario

Il signor Carl Merkel possiede una piccola attività che vende dispositivi mobili chiamata "CM Mobiles" nella città di Harrow, vicino a Londra. Vuole creare un database per memorizzare informazioni chiave sugli ordini dei clienti al fine di generare fatture per i suoi clienti, inclusi il nome del cliente, la data dell'ordine, la quantità e il prezzo totale. Questi dati possono essere visualizzati nella seguente tabella delle fatture:

Tabella delle Fatture

| Nome del cliente | Data dell'ordine | Quantità del prodotto | Prezzo totale |
|------------------|------------------|------------------------|---------------|
| Mark Dani         | 25/03/2022      | 5                      | 750.50        |
| Karl Masonry      | 22/03/2022      | 4                      | 600.75        |
| Jack Raymond      | 20/03/2022      | 3                      | 978.00        |


Diagramma che mostra il file README nella pagina del corso.

Istruzioni

Prova a svolgere i seguenti compiti prima di continuare, così potrai verificare e confrontare le tue risposte con la soluzione.

Devi scrivere un'istruzione SQL "CREATE TABLE" con attributi e tipi di dati pertinenti.

1. Identifica le colonne e definisci il tipo di dati per ciascuna colonna della tabella.

2. Scrivi un'istruzione SQL completa per creare la tabella delle fatture all'interno del database "cm_devices".

Identificazione delle colonne della tabella e dei tipi di dati

Devi progettare la tabella delle fatture, che è composta da diversi tipi di colonne che memorizzeranno tipi di dati differenti. Come sviluppatore del database, dovresti scegliere un tipo di dati che soddisfi il tipo di dati previsto che verrà memorizzato nella colonna, basandoti sulle seguenti linee guida:

- Se ci si aspetta che la colonna memorizzi dati numerici, è necessario scegliere un tipo di dati numerico come INT o DECIMAL.
- Se ci si aspetta che la colonna memorizzi dati alfabetici o alfanumerici, è necessario scegliere un tipo di dati di stringa come CHAR o VARCHAR.
- Se ci si aspetta che la colonna memorizzi dati di data, è necessario scegliere il tipo di dati di data.

Prendi le colonne della tabella delle fatture una per una e applica le linee guida precedenti:

1. Nome del cliente: il tipo di dati di stringa sarebbe molto adatto per il nome del cliente, poiché ci aspettiamo che memorizzi dati con caratteri alfabetici. In questo caso, possiamo utilizzare VARCHAR poiché ci aspettiamo di avere nomi di clienti con lunghezze diverse.

2. Data dell'ordine: il tipo di dati di data sarebbe molto adatto per la data dell'ordine, poiché ci aspettiamo che memorizzi dati con date.

3. Quantità del prodotto: il tipo di dati intero sarebbe molto adatto per la quantità dell'ordine, poiché ci aspettiamo che memorizzi numeri interi di dati.

4. Prezzo totale: il tipo di dati decimale sarebbe molto adatto per il prezzo del prodotto, poiché ci aspettiamo che memorizzi dati numerici con frazioni.

Creazione della tabella delle fatture in SQL

Nota: Devi avere un database in modo da poter creare la tabella al suo interno. Se non ne hai ancora uno, segui i passaggi descritti di seguito per creare il database CM Mobiles.

1. Digita la seguente istruzione SQL nell'editor terminale SQL:

```sql
CREATE DATABASE cm_devices;
```

2. Premi Invio per eseguire l'istruzione di creazione del database.

3. Assicurati di selezionare il database da utilizzare digitando la seguente istruzione SQL e premi Invio:

```sql
USE cm_devices;
```

Seleziona il database corretto digitando Use cm_devices;
Crea l'istruzione SQL come segue:

1. Scrivi un'istruzione SQL che contenga il comando CREATE TABLE seguito dal nome della tabella, che è "invoice" in questo caso.

2. Aggiungi una parentesi aperta per definire le colonne delle tabelle, inclusi il nome del cliente, la data dell'ordine, la quantità e il prezzo totale.

3. Assegna a ogni colonna un tipo di dati adeguato come descritto in precedenza.

4. Aggiungi una parentesi chiusa e un punto e virgola alla fine dell'istruzione SQL come segue:

```sql
CREATE TABLE invoice (customerName VARCHAR(50), orderDate DATE, quantity INT, price DECIMAL);
```

Premi Invio per eseguire la query

Se hai seguito correttamente tutti i passaggi, dovresti ora essere in grado di vedere la tabella delle fatture creata all'interno del database cm_devices digitando:

```sql
SHOW tables;
```

Premi Invio per eseguire la query. Il risultato mostra le tabelle all'interno del database cm_devices.

Risultato della query
Se desideri verificare la struttura della tabella delle fatture, digita la seguente istruzione SQL e premi Invio.

```sql
SHOW columns FROM invoice;
```

Questa istruzione mostra la struttura della tabella delle fatture.

Struttura della tabella delle fatture

In questo esercizio, hai imparato come scegliere tipi di dati adatti per una varietà di colonne.

Ecco un compito aggiuntivo (opzionale) per testare le tue competenze.

Compito aggiuntivo (opzionale)

Il signor Carl ha bisogno di una nuova tabella per memorizzare i dettagli di contatto di ciascun cliente, inclusi il numero di

 conto del cliente, il numero di telefono del cliente e l'indirizzo email del cliente.

Devi scegliere un tipo di dati pertinente per ciascuna delle colonne.

Soluzione:

- Numero di conto: INTEGER
- Numero di telefono: INTEGER
- Email: VARCHAR
