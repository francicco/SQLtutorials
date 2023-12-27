# Obiettivo

L'obiettivo di questo esercizio è imparare come lavorare con i valori predefiniti in un database. Cio' ti permettera' di praticare l'utilizzo dei valori predefiniti utilizzando il vincolo di default in SQL.

## Scenario

Il signor Carl Merkel possiede una piccola attività che vende dispositivi mobili chiamata "CM Mobiles" a Harrow, Londra. Vuole creare un database per memorizzare informazioni chiave sugli indirizzi dei clienti, compresi l'ID del cliente, la via, il codice postale e il nome della città. L'elenco degli indirizzi per i clienti attuali di CM Mobiles è indicato nella tabella degli indirizzi qui sotto.

Tabella degli Indirizzi

| Customer ID | Street         | Postcode | Town      |
|-------------|----------------|----------|-----------|
| 1           | 10 Carsten Rd. | HA3 0AE  | Harrow    |
| 2           | 15 Denise Ave. | HA3 0AE  | Harrow    |
| 3           | 13 Merkel Ave. | HA3 0AE  | Harrow    |
| 4           | 12 Carsten Rd. | HA3 0AE  | Harrow    |
| 5           | 15 Hellen way  | HA3 0AE  | Harrow    |
| 6           | 13 Carsten Rd  | HA3 0AE  | Harrow    |
| 7           | 11 Malika Rd.  | NW9 0AX  | Kingsbury |



## Istruzioni

Crea un'istruzione SQL con attributi e vincoli rilevanti come segue:

1. Identifica la colonna che richiede valori predefiniti.

2. Scrivi un'istruzione SQL completa per creare la tabella degli indirizzi con i vincoli pertinenti.

Prova a svolgere i compiti prima di continuare, in modo da poter verificare e confrontare le tue risposte con la soluzione.

Creazione della tabella

Identificare la colonna con valori predefiniti.

Nota che la tabella degli indirizzi mostra che la maggior parte dei clienti vive nell'area di Harrow, il che significa che i clienti di CM Mobiles provengono principalmente da questa città.

In questo caso, puoi definire un valore predefinito per la colonna della città come "Harrow" quando crei la tabella degli indirizzi.

Ciò risparmierebbe al signor Carl la necessità di inserire "Harrow" ripetutamente nel campo della città per ogni nuovo record del cliente, poiché verrà automaticamente compilato con il valore predefinito "Harrow". Naturalmente, ciò presuppone che nessun altro valore sia stato inserito nella tabella.

Nota: Devi avere un database per creare la tabella al suo interno. Se non ne hai ancora uno, vedi sotto come creare il database CM Mobiles.

1. Digita la seguente istruzione SQL nell'editor terminale SQL.

```sql
CREATE DATABASE cm_devices;
```

2. Premi Invio per eseguire l'istruzione di creazione del database.

3. Assicurati di selezionare il database da utilizzare digitando la seguente istruzione SQL e premi Invio.

```sql
USE cm_devices;
```

4. Crea una tabella.

4.1 Scrivi il comando SQL CREATE TABLE seguito dal nome della tabella, in questo caso "address".

4.2 Apri una parentesi per definire le colonne della tabella, inclusi l'ID del cliente, la via, il codice postale e la città. Ogni colonna deve essere assegnata a un tipo di dati adatto, come hai appreso in video ed esercizi precedenti.

4.3 Utilizza la parola chiave SQL "DEFAULT" per dichiarare il valore predefinito preimpostato.

4.4 Una volta definite tutte le colonne necessarie, aggiungi una parentesi di chiusura e un punto e virgola alla fine dell'istruzione SQL come segue:

```sql
CREATE TABLE address(id int NOT NULL, street varchar(255), postcode varchar(10), town varchar(30) DEFAULT "Harrow");
```

La parola chiave DEFAULT utilizzata in questa istruzione è seguita dal valore predefinito "Harrow" per la colonna del nome della città nella tabella degli indirizzi. In questo caso, se il signor Carl Merkel vuole inserire dati in questa tabella, allora non c'è bisogno di digitare "Harrow" per nessun cliente che vive in questa città, poiché verrà inserito automaticamente.

5. Esegui la query premendo Invio.

6. Se desideri verificare la struttura della tabella degli indirizzi, digita la seguente istruzione SQL e premi Invio:

```sql
SHOW columns FROM address;
```

Ciò mostra tutte le colonne della tabella degli indirizzi con i tipi di dati e il vincolo di default "Harrow".

Tutte le colon

ne della tabella degli indirizzi con i tipi di dati e il vincolo di default "Harrow" vengono visualizzate.

In questo esercizio, hai imparato come utilizzare il vincolo di default per imporre un valore predefinito specifico, che è molto utile per colonne che si aspettano di contenere gli stessi dati.

Ecco un compito aggiuntivo (opzionale) per testare le tue competenze.

Compito aggiuntivo (opzionale)

Il signor Carl Merkel nota che la maggior parte dei clienti proviene dallo stesso codice postale, ovvero "HA97DE".

Devi scrivere nuovamente l'istruzione SQL per dichiarare sia il codice postale che il nome della città con valori predefiniti.

Ricorda di eliminare la tabella degli indirizzi prima di crearne una nuova.

Per eliminare la tabella, basta digitare: DROP TABLE Address;.

Soluzione:

```sql
CREATE TABLE Address (id int NOT NULL,  street VARCHAR(255), postcode VARCHAR(10) DEFAULT "HA97DE", town VARCHAR(30) DEFAULT "Harrow");
```
