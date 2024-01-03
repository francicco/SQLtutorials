# Obiettivo

Lo scopo di questo esercizio è imparare come lavorare con valori numerici in un database. L'obiettivo sara' quello di essere in grado di dlavorare con tipi di dati numerici in SQL.

## Scenario

Il signor Carl Merkel possiede una piccola azienda chiamata CM Mobiles che vende dispositivi mobili. Vuole creare un database per memorizzare informazioni chiave sui dispositivi mobili. Queste informazioni includono:
- L'ID o numero del dispositivo,
- Il nome del dispositivo,
- E il prezzo del dispositivo.

Questa informazione è visualizzata nella seguente tabella:

Tabella Dispositivi Mobili

| Device ID | Device Name | Price   |
|-----------|-------------|---------|
| 1         | iPhone XR 1 | 1500.50 |
| 2         | Samsung SX  | 1200.50 |
| 3         | Nokia 730   | 1050.00 |

Nota: Devi completare questo esercizio all'interno di MySQL. Se hai dubbi su come accedervi, consulta il materiale del corso.

### Istruzioni



*Compito 1: Creare un database chiamato cm_devices*

1. Digita la seguente istruzione SQL nell'editor terminale SQL:

```sql
CREATE DATABASE cm_devices;
```

2. Premi Invio per eseguire l'istruzione di creazione del database.

3. Assicurati di selezionare il database in cui vuoi creare la tabella digitando la seguente istruzione SQL:

```sql
USE cm_devices;
```

4. La parola chiave SQL 'USE' è utilizzata per selezionare un database in MySQL seguita dal nome del database. Premi Invio per eseguire la query. L'output sarà "Database changed":



*Compito 2: Creare un'istruzione SQL con attributi e tipi di dati rilevanti*

1. Identifica un nome adatto per la tabella in cui vuoi memorizzare i dati sui dispositivi mobili. In questo caso, puoi chiamare la tabella "devices".

2. Identifica il tipo di dati per ciascuna colonna della tabella.

Sulla base dei requisiti di CM Mobiles, la tabella dei dispositivi mobili deve contenere tre colonne come segue:

- Una colonna chiamata "Device ID" che memorizza numeri interi. In questo caso, dovresti utilizzare INT come tipo di dati.
- Una seconda colonna chiamata "Device name" che si prevede memorizzerà dati come un valore alfanumerico simile a una stringa. Ad esempio, iPhone XR 1.
- E una terza colonna chiamata "Price". Questa colonna finale dovrebbe memorizzare dati numerici con possibili valori frazionari. In questo caso, dovresti utilizzare il tipo di dati DECIMAL. Con il tipo di dati decimal, non c'è alcun problema a memorizzare un numero intero perché la parte frazionaria è separata da un punto decimale con 00 sul lato destro del numero. Ciò è indicato nella terza riga della tabella dei dispositivi mobili, dove il prezzo è 1050.00 (che è equivalente a 1050).

3. Scrivi un'istruzione SQL completa per creare la tabella dei dispositivi mobili all'interno del database cm_devices.

3.1 Scrivi l'istruzione SQL che contiene il comando CREATE TABLE seguito dal nome della tabella, che è "devices" in questo caso.

3.2 Apri una parentesi per definire le colonne della tabella, inclusi l'ID del dispositivo, il nome del dispositivo e il prezzo.

3.4 Definisci un tipo di dati appropriato per ciascuna colonna come segue:
- La colonna dell'ID del dispositivo con tipo di dati intero.
- La colonna del prezzo con tipo di dati decimale.
- Il nome del dispositivo con VARCHAR con un limite massimo di 50 caratteri.

3.5 Aggiungi una parentesi di chiusura e un punto e virgola alla fine dell'istruzione SQL. L'istruzione completa dovrebbe replicare la seguente sintassi:

```sql
CREATE TABLE devices (deviceID int, deviceName varchar(50), price decimal);
```

3.6 Esegui la query premendo Invio.

Se hai seguito tutti i passaggi correttamente, dovresti ora essere in grado di accedere alla tabella dei dispositivi che è stata creata all'interno del database cm_mobiles digitando:

```sql
SHOW tables;
```

3.7 Premi Invio.

Lo statement SHOW tables mostra tutte le tabelle. La tabella Devices è ora visibile.
Per controllare la struttura della tabella devices, digita la seguente istruzione SQL e premi Invio:

```sql
SHOW columns FROM devices;
```

Questo mostra tutte le colonne e i tipi di dati della tabella devices.

Vengono visualizzate tutte le colonne e i tipi di dati della tabella devices.

In questo esercizio, hai esercitato come definire tipi di dati numerici in un database. Ecco un compito aggiuntivo (opzionale) per testare le tue competenze.

Compito aggiuntivo (opzionale)

Il signor Merkel desidera creare un'altra tabella di base nel database per memorizzare dati sullo stock dei dispositivi, inclusi l'ID del dispositivo, la quantità disponibile nello stock e il costo totale disponibile della quantità. Questa tabella di base è mostrata nella tabella sottostante, con ogni colonna che mostra l'ID del dispositivo, la quantità in stock e il prezzo totale.

Tabella Stock

| Device ID | Quantity | Total price |
|-----------|----------|-------------|
| 1         | 5        | 5000.75     |
| 2         | 3        | 3500.50     |

Sulla base della tabella e delle informazioni fornite, completa quanto segue:

1. Identifica un nome appropriato per la tabella da creare, dati i dati forniti.

2. Identifica le colonne che dovrebbero essere presenti in questa tabella e definiscile con i tipi di dati appropriati.

3. Scrivi l'intera istruzione SQL che crea la tabella e le colonne.

Soluzione:

1: Nome tabella: Stock

2: Device id INT

   Quantità INT

   Costo totale Decimal

3: 

```sql
CREATE TABLE stock (deviceID int, quantity int, totalPrice decimal);
```
