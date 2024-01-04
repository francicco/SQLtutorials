# Normalizzazione dei dati
Il processo di normalizzazione mira a ridurre al minimo le duplicazioni dei dati, evitare errori durante le modifiche dei dati e semplificare le query dei dati dal database. Le tre forme fondamentali di normalizzazione sono conosciute come:

1. Prima forma di normalizzazione (1NF)
2. Seconda forma di normalizzazione (2NF)
3. Terza forma di normalizzazione (3NF)


In questa lettura, imparerai come applicare le regole che assicurano che un database soddisfi i criteri di queste tre forme normali.

L'esempio seguente include dati fittizi richiesti da un gruppo chirurgico medico con sede a Londra per generare rapporti pertinenti. I medici lavorano in diverse regioni e vari consigli a Londra. E una volta che i pazienti prenotano un appuntamento, viene loro assegnato un ID slot presso il loro ambulatorio locale. Potrebbero esserci più ambulatori nello stesso consiglio ma con diversi codici postali, dove uno o più consigli appartengono a una particolare regione. Ad esempio, East o West London.

|Doctor ID | Doctor name | Region  |Patient ID|  Patient name|  Surgery Number|  Surgery council|    Postcode|Slot ID|   Total Cost|
|----------|-------------|---------|----------|--------------|----------------|-----------------|------------|-------|-------------|
|D1  |Karl  |West London |P1, P2, P3 |Rami, Kim, Nora | 3  |Harrow | HA9SDE|A1, A2, A3 |1500, 1200, 1600  |
|D1  |Karl  |East London  |P4, P5  |Kamel  |4  |Hackney, Sami  |E1 6AW|A1, A2  |2500 1000  |
|D2  |Mark  |East London  |P5, P6  |Sami, Norma  |4  |Hackney   |E1 6AW|A3, A4  |1500 2000  |
|D2  |Mark  |West London  |P7, P1 |Rose  |5|Harrow, Rami   |HA862E|A4, A5  |1000, 1500 |

I dati elencati nella tabella sono in forma non normalizzata. In molti casi compaiono gruppi ripetuti di dati, ad esempio nomi di medici, regioni e comuni. Sono inoltre presenti più istanze di dati archiviate nella stessa cella, ad esempio con le colonne del nome del paziente e del costo totale. Ciò rende difficile l'aggiornamento e l'interrogazione dei dati. Inoltre, non è semplice scegliere una chiave univoca e assegnarla come chiave primaria.

Questa tabella non normalizzata può essere scritta in formato SQL come segue:

```sql
CREATE TABLE Surgery
    (DoctorID VARCHAR(10),
    DoctorName VARCHAR(50),
    Region VARCHAR(20),
    PatientID VARCHAR(10),
    PatientName VARCHAR(50),
    SurgeryNumber INT, Council VARCHAR(20),
    Postcode VARCHAR(10),
    SlotID VARCHAR(5),
    TotalCost Decimal);
```

## Prima forma di normalizzazione
Per semplificare la struttura dei dati della tabella chirurgica, applichiamo le regole della prima forma di normalizzazione per far rispettare la regola di atomicità dei dati ed eliminare gruppi di dati ripetuti non necessari. La regola di atomicità dei dati significa che puoi avere un'unica istanza di valore dell'attributo di colonna in qualsiasi cella della tabella.

Il problema di atomicità esiste solo nelle colonne di dati relativi ai pazienti. Pertanto, è importante creare una nuova tabella per i dati del paziente per risolvere questo problema. In altre parole, è possibile organizzare tutti i dati relativi all'entità del paziente in una tabella separata, in cui ogni cella di colonna contiene solo un'unica istanza di dati, come illustrato nell'esempio seguente:

|Patient ID  |Patient name  |Slot ID   |Total Cost   |
|-|-|-|-|
|P1  |Rami  |A1  |1500  |
|P2  |Kim  |A2  |1200  |
|P3  |Nora  |A3  |1600  |
|P4  |Kamel  |A1  |2500  |
|P5  |Sami  |A2  |1000  |
|P6|Norma  |A5  |2000  |
|P7|Rose  |A6  |1000  |

Questa tabella include una singola istanza di dati in ogni cella, il che la rende molto più semplice da leggere e comprendere. Tuttavia, la tabella dei pazienti richiede due colonne, l'ID paziente e l'ID slot, per identificare ogni record in modo univoco. Ciò significa che è necessaria una chiave primaria composita in questa tabella. Per creare questa tabella in SQL puoi scrivere il seguente codice:

```sql
CREATE TABLE Patient
 (PatientID VARCHAR(10) NOT NULL,
  PatientName VARCHAR(50),
  SlotID VARCHAR(10) NOT NULL,
  TotalCost Decimal,
  CONSTRAINT PK_Patient
  PRIMARY KEY (PatientID, SlotID));
```

Una volta rimosse le proprietà del paziente dalla tabella principale, rimangono solo le colonne dell'ID del medico, del nome, della regione, del numero di intervento, del consiglio e del codice postale nella tabella.

| ID del Medico | Nome del Medico | Regione      | Numero di Intervento | Consiglio | Codice Postale |
|---------------|-----------------|--------------|----------------------|-----------|----------------|
| D1            | Karl            | West London  | 3                    | Harrow    | HA9SDE         |
| D1            | Karl            | East London  | 4                    | Hackney   | E1 6AW         |
| D2            | Mark            | West London  | 4                    | Hackney   | E1 6AW         |
| D2            | Mark            | East London  | 5                    | Harrow    | HA862E         |

Potresti aver notato che la tabella contiene anche gruppi ripetuti di dati in ciascuna colonna. Puoi risolvere questo problema separando la tabella in due tabelle di dati: la tabella dei medici e la tabella delle operazioni, dove ogni tabella tratta un'entità specifica.

**Tabella dei Medici**    
| ID del Medico | Nome del Medico |
|---------------|-----------------|
| D1            | Karl            |
| D2            | Mark            |


**Tabella degli interventi**
|Numero Intervento | Regione       | Consiglio | Codice Postale|
|-|-|-|-|
|3                 | West London   | Harrow    | HA9SDE|
|4                 | East London   | Hackney   | E1 6AW|
|5                 | West London   | Harrow    | HA862E|

Nella tabella dei medici, è possibile identificare l'ID del medico come chiave primaria composta da una sola colonna. Questa tabella può essere creata in SQL scrivendo il seguente codice:

```sql
CREATE TABLE Doctor 
  (DoctorID VARCHAR(10), 
  DoctorName VARCHAR(50), PRIMARY KEY (DoctorID)
);
```

Analogamente, la tabella delle operazioni può avere il numero di intervento come chiave primaria composta da una sola colonna. La tabella delle operazioni può essere creata in SQL scrivendo il seguente codice:

```sql
CREATE TABLE Surgery 
 (SurgeryNumber INT NOT NULL, 
 Region VARCHAR(20), Council VARCHAR(20), 
 Postcode VARCHAR(10), PRIMARY KEY (SurgeryNumber)
);
```

Applicando la regola di atomicità e rimuovendo i gruppi di dati ripetuti, il database ora rispetta la prima forma di normalizzazione.

## Seconda forma di normalizzazione
Nella seconda forma di normalizzazione, è necessario evitare relazioni di dipendenza parziale tra i dati. La dipendenza parziale si riferisce alle tabelle con una chiave primaria composita, cioè una chiave composta da una combinazione di due o più colonne, in cui il valore di un attributo non chiave dipende solo da una parte della chiave composta.

Poiché la tabella dei pazienti è l'unica che include una chiave primaria composita, è sufficiente guardare solo a questa tabella.

| ID Paziente | Nome Paziente | ID Slot | Costo Totale |
|-------------|---------------|---------|--------------|
| P1          | Rami          | A1      | 1500         |
| P2          | Kim           | A2      | 1200         |
| P3          | Nora          | A3      | 1600         |
| P4          | Kamel         | A1      | 2500         |
| P5          | Sami          | A2      | 1000         |
| P5          | Sami          | A3      | 1000         |
| P6          | Sami          | A4      | 1500         |
| P7          | Norma         | A5      | 2000         |
| P8          | Rose          | A6      | 1000         |
| P1          | Rami          | A7      | 1500         |

Nella tabella dei pazienti, è necessario verificare se ci sono attributi non chiave che dipendono solo da una parte della chiave composita. Ad esempio, il nome del paziente è un attributo non chiave e può essere determinato utilizzando solo l'ID del paziente.

Analogamente, è possibile determinare il costo totale utilizzando solo l'ID dello slot. Questo è chiamato dipendenza parziale, che non è consentita nella seconda forma di normalizzazione. Ciò perché tutti gli attributi non chiave dovrebbero essere determinati utilizzando entrambe le parti della chiave composita, non solo una di esse.

Questo può essere risolto suddividendo la tabella dei pazienti in due tabelle: la tabella dei pazienti e la tabella degli appuntamenti. Nella tabella dei pazienti è possibile mantenere l'ID del paziente e il nome del paziente.

Tabella dei pazienti:

| ID Paziente | Nome Paziente |
|-------------|---------------|
| P1          | Rami          |
| P2          | Kim           |
| P3          | Nora          |
| P4          | Kamel         |
| P5          | Sami          |
| P7          | Norma         |
| P8          | Rose          |

La nuova tabella dei pazienti può essere creata in SQL utilizzando il seguente codice:

```sql
CREATE TABLE Paziente
  (IDPaziente VARCHAR(10) NOT NULL,
   NomePaziente VARCHAR(50),
   PRIMARY KEY (IDPaziente)
  );
```

Tuttavia, nella tabella degli appuntamenti, è necessario aggiungere una chiave unica per garantire di avere una chiave primaria che possa identificare ogni record unico nella tabella. Pertanto, l'attributo ID appuntamento può essere aggiunto alla tabella con un valore unico in ogni riga.

![image](https://github.com/francicco/SQLtutorials/assets/9006870/e13c289d-01a2-4a12-ad70-1bfc898b917f)

The new appointments table can be created in SQL using the following code:

```sql
CREATE TABLE Appointments 
 (AppointmentID INT NOT NULL, 
  SlotID VARCHAR(10),  
  TotalCost Decimal, PRIMARY KEY (AppointmentID)
 );
```
You have removed the partial dependency, and all tables conform to the first and second normal forms.

## Terza forma di normalizzazione
Perché una relazione in un database sia nella terza forma di normalizzazione, deve già trovarsi nella seconda forma di normalizzazione (2NF). Inoltre, non deve avere alcuna dipendenza transitiva. Ciò significa che ogni attributo non chiave nella tabella delle operazioni non deve dipendere funzionalmente da un altro attributo non chiave nella stessa tabella. Nella tabella delle operazioni, il codice postale e il consiglio sono attributi non chiave e il codice postale dipende dal consiglio. Pertanto, se cambi il valore del consiglio, devi cambiare anche il codice postale. Questa dipendenza transitiva non è consentita nella terza forma di normalizzazione.

|Surgery number  |Region  |Surgery council    |Postcode|
|----------------|--------|-------------------|--------|
|3  |West London  |Harrow  |HA9SDE|
|4  |East London  |Hackney  |E1 6AW| 
|5|West London  |Harrow  |HA862E|

In altre parole, la modifica del valore del comune nella tabella sopra ha un impatto diretto sul valore del codice postale, poiché ciascun codice postale in questo esempio appartiene a un comune specifico. Questa dipendenza transitiva non è consentita nella terza forma di normalizzazione. Per risolvere il problema puoi dividere questa tabella in due tabelle: una per la regione con la città e una per l'ambulatorio.

**Tabella degli indirizzi postali**
|Surgery number  |Postcode|
|-|-|
|3  |HA9SDE|
|4  |E1 6AW |
|5|HA862E|

La nuova tabella delle posizioni dell'intervento può essere creata in SQL utilizzando il seguente codice:
```sql
CREATE TABLE Location
 (SurgeryNumber INT NOT NULL,
  Postcode VARCHAR(10), PRIMARY KEY (SurgeryNumber));
```

**Council table**
|Surgery council    |Region  |
|-|-|
|Harrow  |West London  |
|Hackney  |East London |

La nuova tabella del consiglio operatorio può essere creata in SQL utilizzando il seguente codice:
```sql
CREATE TABLE Council
 (Council VARCHAR(20) NOT NULL,
  Region VARCHAR(20), PRIMARY KEY (Council));
```

Ciò garantisce che il database sia conforme alla prima, seconda e terza forma di normalizzazione. Il diagramma seguente illustra le fasi attraverso le quali i dati passano dalla forma non normalizzata alla prima forma di normalizzazione, alla seconda forma di normalizzazione e infine alla terza forma di normalizzazione.
![image](https://github.com/francicco/SQLtutorials/assets/9006870/c8f1fa61-caf5-429c-89fe-d3413e9c8ad2)

Tuttavia, è importante collegare tutte le tabelle insieme per garantire di avere tabelle ben organizzate e correlate nel database. Questo può essere fatto definendo le chiavi esterne nelle tabelle.
![image](https://github.com/francicco/SQLtutorials/assets/9006870/d9ee4239-d1d5-45c8-b4c3-263114c1814d)

La terza forma di normalizzazione è in genere sufficiente per affrontare le tre sfide anomale – anomalie di inserimento, aggiornamento e cancellazione – che il processo di normalizzazione mira ad affrontare. Il completamento della terza forma di normalizzazione nella progettazione di un database aiuta a sviluppare un database di facile accesso e interrogazione, ben strutturato, ben organizzato, coerente e senza inutili duplicazioni di dati.
