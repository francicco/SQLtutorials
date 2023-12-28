# Obiettivo

In questo esercizio imparerai come praticare la creazione di una tabella in un database da zero. L'obiettivo dell'esercizio è creare una tabella in un database per lo scenario fornito. Successivamente, esaminerai la soluzione per controllare e confrontare il tuo design e l'implementazione della tabella.

## Scenario

Il signor Erik Anderson ha creato un nuovo club di calcio per ragazzi sotto i 16 anni. Deve creare una tabella per memorizzare i dati personali di base dei giocatori, tra cui numero di identità, nome ed età.

## Istruzioni

Prova a svolgere i compiti seguenti prima di continuare, in modo da poter controllare e confrontare le tue risposte con la soluzione.

**Compito 1: Crea un database chiamato football club.**

1. Non è possibile creare tabelle se non esiste un database pertinente disponibile per creare tabelle al suo interno. Pertanto, creiamo un nuovo database per i dati del signor Anderson digitando la seguente istruzione SQL nell'editor di codice SQL.

```sql
CREATE DATABASE football_club;
```

2. Premi Invio per eseguire la creazione del database football_club.

3. Assicurati di selezionare il database da utilizzare digitando la seguente istruzione SQL e premendo Invio:

```sql
USE football_club;
```

**Compito 2: Crea la tua tabella.**

1. Scrivi un'istruzione SQL che contenga il comando CREATE TABLE seguito dal nome della tabella. Players è un nome adatto in questo caso.

2. Apri una parentesi per definire le colonne della tabella:
   - Player ID
   - Player name
   - Player age

3. Assegna a ciascuna colonna un tipo di dati adatto. In questo caso, puoi scegliere i seguenti tipi di dati adatti:
   - Player ID: INT
   - Player name: VARCHAR(50)
   - Player age: INT

4. Una volta definite tutte le colonne necessarie, aggiungi una parentesi chiusa e un punto e virgola alla fine dell'istruzione SQL come segue:

```sql
CREATE TABLE players (playerID int, playerName varchar(50), age int);
```

5. Premi Invio per eseguire l'istruzione SQL.

   L'immagine sotto mostra l'output dopo l'esecuzione dell'istruzione CREATE TABLE players.

   ![Screenshot dell'istruzione SQL CREATE TABLE](immagine_sql_create_table.png)

   Se hai seguito correttamente tutti i passaggi, dovresti ora vedere la tabella dei giocatori creata all'interno del database football_club digitando la seguente istruzione show tables:

```sql
SHOW tables;
```

6. Premi Invio per visualizzare la tabella dei giocatori all'interno del database football_club. Potrebbero essere presenti altre tabelle se ne hai già create altre all'interno di questo database.

   L'immagine sotto mostra l'output dell'istruzione SHOW tables. La tabella dei giocatori è ora visibile.

   ![Screenshot dell'istruzione SQL SHOW tables](immagine_sql_show_tables.png)

In questo esercizio, hai praticato come creare una tabella di base all'interno di un database. Ecco un compito aggiuntivo facoltativo per testare le tue abilità.

**Compito aggiuntivo (facoltativo)**

Il signor Anderson desidera creare un'altra tabella per registrare informazioni sulle partite che la squadra giocherà, inclusi gameID, il punteggio di ogni partita e le date in cui verranno giocate. Il tuo compito è creare questa tabella per il club di calcio.

**Soluzione**

Scrivi la seguente istruzione SQL e premi Invio per eseguirla:

```sql
CREATE TABLE games(gameID INT, gameDate DATE, score INT);
```
