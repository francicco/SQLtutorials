# SQL: Esplorazioni dei dati: uso di `SELECT`.

### Per prima cosa possiamo iniziare con il chiederci quanti geni sono stati usati in questo esperimento?
### Come possiamo estrarre questo dato?

```sql
SELECT count(`OGid`) FROM `AnalisiOG`;
```

<details>
<summary>Soluzione</summary>
```
count(`OGid`)
2487
```
</details>

### Allo stesso modo, quali geni sono sotto intensificazione?
<details>
<summary>Soluzione</summary>
	
```sql
SELECT `OGid`
FROM `AnalisiOG`
WHERE `Intensified` = 1;
```
</details>

### Quali e Quanti sotto intensificazione e convergenza?
<details>
<summary>Soluzione</summary> 

```sql
SELECT `OGid` FROM `AnalisiOG`
WHERE `Intensified` = 1
AND `CSUBST` = 1;
```
</details>

<details>
<summary>Soluzione</summary> 

```sql
SELECT count(`OGid`) FROM `AnalisiOG`
WHERE `Intensified` = 1
AND `CSUBST` = 1;
```
</details> 

### Come possiamo raggruppare tutti i campi in una sola query?
<details>
<summary>Soluzione</summary> 

```sql
SELECT sum(`BUSTEDPH`) AS sum_BUSTED,
	sum(`Relaxed`) AS sum_Relaxed, 
	sum(`Intensified`) AS sum_Intensified,
	sum(`CSUBST`) AS sum_CSUBST
FROM `AnalisiOG`;
````
</details>


### Quanti CNEE ci sono per cromosoma?
<details>
<summary>Soluzione</summary>	

```sql
SELECT Chr, COUNT(*) AS chr_count FROM CNEEtable GROUP BY Chr ORDER BY chr_count DESC;
```
</details>


<details>
<summary>Soluzione</summary> 
	
```sql
SELECT COUNT(BUSTEDPH) AS BUSTEDPH_count FROM `AnalisiOG`;
```
</details>

### Vediamo un altra query piu' complessa. Ad esempio vorrei voler sapere quale sia la lunghezza totale dei CNEE per cromosoma.
In altre parole la somma della lunghezza di ogni CNEE per ogni cromosoma.

Come vogliamo affrontare il problema? Posso pensare di spezzare il problema in problemi piu' semplici, il calassico approccio del [Divide & Conquer](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm#:~:text=The%20divide%2Dand%2Dconquer%20paradigm,to%20solve%20the%20given%20problem.)
In questo caso, quali potrebbero essere i sotto problemi piu' semplici?

<details>
<summary>Sotto problema 1:</summary> 
Calcolare la lunghezza di ogni CNEE

<details>
<summary>Query problema 1:</summary> 

```sql
SELECT `Chr`, (End - Start + 1) AS lunghezza 
    FROM CNEEtable
```
</details>
</details>

<details>	
<summary>Sotto problema 2:</summary> 
Sommare i singoli valori raggruppando per cromosoma

<details>
<summary>Query problema 2:</summary> 

```sql
SELECT `Chr`, SUM(lunghezza)
FROM ()
GROUP BY `Chr`;
```

### Ovviamente non abbiamo una tabella gia' fatta per questa query, ma possiamo usare la query precedente come "tabella" di input.
Per fare questa ci serve trasformare la `SELECT` in un oggetto con la clausola `AS`.

```sql
SELECT `Chr`, SUM(lunghezza)
FROM (SELECT `Chr`, (End - Start + 1) AS lunghezza 
    FROM CNEEtable) AS subquery
GROUP BY `Chr`;
```
</details>
</details>

### Possiamo effettuare tutte le operazioni base, ad esempio, oltre alla somma, la media.
Ad esempio la media delle `NumAccelM1` nella tabella `phyloAccCNEE`

```sql
SELECT AVG(`NumAccelM1`) AS average_NumAccelvalue
FROM `phyloAccCNEE`
WHERE `Model` = 'M1';
```

O la mediana... 

La formula della [mediana](https://en.wikipedia.org/wiki/Median)
![image](https://github.com/francicco/SQLtutorials/assets/9006870/17f05f4d-b93a-4ce5-b4c7-5c350c98bd39)

<details>
<summary>	Query problema 2:</summary> 

```sql
SELECT AVG(median_value) AS median 
FROM (
    SELECT `NumAccelM1`, row_num, total_rows,
           CASE
               WHEN total_rows % 2 = 1 THEN `NumAccelM1`
               WHEN row_num IN ((total_rows + 1) / 2, (total_rows + 2) / 2) THEN `NumAccelM1`
               ELSE NULL
           END AS median_value
    FROM (
        SELECT `NumAccelM1`,
               ROW_NUMBER() OVER (ORDER BY `NumAccelM1`) AS row_num,
               COUNT(*) OVER () AS total_rows
        FROM `phyloAccCNEE`
        WHERE `Model` = 'M1'
    ) AS subquery
) AS med;
```



E la mediana

SELECT 
  'field_name' AS field_name,
  AVG(median_value) AS median_value
FROM (
  SELECT 
    field_name AS median_value
  FROM 
    (SELECT 
      field_name, 
      ROW_NUMBER() OVER (ORDER BY field_name) AS row_num,
      COUNT(*) OVER () AS total_rows
    FROM 
      your_table_name
    ORDER BY 
      field_name) AS sorted
  WHERE 
    row_num IN ((total_rows + 1) / 2, (total_rows + 2) / 2)
) AS med;



### - `SELECT` per l'uso di *Joins* e *Unions*

Il comando `SELECT` puo' anche essere usato per creare *join*: che aiutano a combinare i dati di più tabelle in base a una colonna correlata tra loro; e le *union*: che invece consentono di combinare i set di risultati di due o più istruzioni `SELECT`.
Entrambi sono fondamentali per sfruttare tutta la potenza delle query SQL.

![Group 7141 (1)](https://github.com/francicco/SQLtutorials/assets/9006870/261aaad4-63b9-4b58-9cc0-37da2a12f00e)

Tipologie di join:
- `INNER JOIN`: restituisce le righe quando c'è una corrispondenza in entrambe le tabelle.
- `LEFT JOIN`: restituisce tutte le righe della tabella di sinistra e le righe corrispondenti della tabella di destra.
- `RIGHT JOIN`: restituisce tutte le righe della tabella di destra e le righe corrispondenti della tabella di sinistra.
- `FULL JOIN`: restituisce tutte le righe quando c'è una corrispondenza in una delle tabelle.

Per illustrare i vari tipi JOIN in SQL, considera di voler estrarre tutti gli CNEE che appartengono alla mia lista di geni ortologhi.

#### `INNER JOIN`:
```sql
SELECT AnalisiOG.*, CNEEtable.*
FROM AnalisiOG 
INNER JOIN CNEEtable 
ON AnalisiOG.OGid = CNEEtable.OGid;
```

Possiamo inoltre *rinominare* le tabelle:

```sql
SELECT A.*, C.*
FROM AnalisiOG AS A
INNER JOIN CNEEtable AS C
ON A.OGid = C.OGid;
```
 ... Ed inserire una condizione con `WHERE`, e scegliere nel dettaglio quali campi selezionare
	
```sql
SELECT A.OGid, A.CSUBST, C.CNEEid
FROM AnalisiOG A
INNER JOIN CNEEtable C
ON A.OGid = C.OGid
WHERE A.CSUBST = 1;
```
	
#### `LEFT JOIN`:

```sql
SELECT A.*, C.*
FROM AnalisiOG A
LEFT JOIN CNEEtable C
ON A.OGid = C.OGid
WHERE A.Relaxed = 1
ORDER BY A.OGid;
```

E' abbastanza intuitivo vedere come il `LEFT JOIN` mostra anche i gruppi orthologhi che non hanno un corrispettivo nella tabella `CNEEtable`, cio' vuol dire che quel gene non presenta alcuna regione conservata nelle sue vicinanze.

Possimo usare questa informazione per cercare ad esempio quali sono gli OG che non hanno nessun CNEE nelle sue regioni regolatorie.
Uso la query precente per mostrare i campi `NULL`...

```sql
SELECT A.OGid, A.Relaxed, C.CNEEid
FROM AnalisiOG A
LEFT JOIN CNEEtable C ON A.OGid = C.OGid
WHERE A.Relaxed = 1
AND C.CNEEid IS NULL;
```

e contare gli OG con i campi `NULL`:
```sql
SELECT SUM(RelaxedNull) AS RelaxedNull
FROM (SELECT A.OGid, A.Relaxed AS RelaxedNull, C.CNEEid
FROM AnalisiOG A
LEFT JOIN CNEEtable C ON A.OGid = C.OGid
WHERE A.Relaxed = 1
AND C.CNEEid IS NULL) AS subquery;
```


#### `RIGHT JOIN`:
Se invece eseguiamo il `RIGHT JOIN`...

```sql
SELECT A.*, C.*
FROM AnalisiOG A
RIGHT JOIN CNEEtable C
ON A.OGid = C.OGid
WHERE A.Relaxed = 1
ORDER BY A.OGid;
```

... quanti match troviamo rispetto al `LEFT JOIN`?

<details>
<summary>Soluzione</summary> 

```sql
SELECT count(A.OGid)
FROM AnalisiOG A
RIGHT JOIN CNEEtable C
ON A.OGid = C.OGid
WHERE A.Relaxed = 1;
```
```sql
SELECT count(A.OGid)
FROM AnalisiOG A
LEFT JOIN CNEEtable C
ON A.OGid = C.OGid
WHERE A.Relaxed = 1;
```
</details>

#### `FULL JOIN`:
La `FULL JOIN` rappresenta l'unione dei due insiemi e in mySQL puo' essere ottenuta con il comando `UNION`

```sql
SELECT *
FROM AnalisiOG A
LEFT JOIN CNEEtable C ON A.OGid = C.OGid
UNION
SELECT *
FROM AnalisiOG A
RIGHT JOIN CNEEtable C ON A.OGid = C.OGid;
```

### - `IN` per l'uso `SELECT` all'interno di altre `SELECT`
la clausola `IN` viene utilizzata per specificare un elenco di valori per il confronto in una clausola WHERE. Consente di filtrare il set di risultati in base al fatto che un valore corrisponda a qualsiasi valore in un elenco specificato.

La clausola IN non è alternativa alla clausola `JOIN`; piuttosto, servono a scopi diversi.

- `JOIN` viene utilizzata per combinare righe di due o più tabelle in base a una colonna correlata tra di loro. Ti consente di recuperare dati correlati da più tabelle.
- la clausola `IN`, invece viene utilizzata per filtrare il set di risultati in base al fatto che un valore corrisponda a qualsiasi valore in un elenco specificato. Viene in genere utilizzato nella clausola `WHERE` per filtrare le righe in base a un elenco di valori specifici.

Sebbene sia JOIN che IN possano essere utilizzati per filtrare i dati, funzionano in modo diverso e vengono utilizzati per scopi diversi:

`JOIN` viene utilizzato per recuperare dati da tabelle correlate combinando righe.
`IN` viene utilizzato per filtrare le righe in base a un elenco specifico di valori.
In alcuni casi, potresti utilizzare sia JOIN che IN insieme per eseguire query più complesse, ad esempio unendo tabelle e quindi filtrando il risultato utilizzando la clausola IN. Ma non sono intercambiabili e il loro utilizzo dipende dai requisiti specifici della tua query.

Vediamo un esempio:

Mettima il caso io voglia estrarre tutti i CNEE che sono presenti nel set di geni che sono sotto intensificazione (`Intensified = 1`) e abbiamo evidenza di convergenza (`CSUBST = 1`). Con il `JOIN` ptrei eseguire un comando di questo tipo:

```sql
SELECT C.*
FROM CNEEtable C
JOIN AnalisiOG A ON C.OGid = A.OGid
WHERE A.Intensified = 1
AND A.CSUBST = 1;
```

Con la clausola `IN` invece, per prima cosa estraggo tutti gli OG che presentano le due condizioni, semplice `SELECT`

<details>
<summary>Soluzione</summary> 

```sql
SELECT `OGid`
FROM `AnalisiOG`
WHERE `Intensified` = 1
AND `CSUBST` = 1;
```
</details>

Ora, senza usare `JOIN` ma usando `WHERE` e `IN`. Creo una condizione per cui verifico se gli `OGid` della tabella `CNEEtable` sono nella `SELECT` (`IN`) che ho creato:

```sql
SELECT C.*
FROM `CNEEtable` C
WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
		WHERE `Intensified` = 1
		AND `CSUBST` = 1);
```

Allo stesso modo, posso creare una lista di questi `OGid` ed estrarre i risutati della tabella `phyloAccCNEE`? 
<details>
<summary>Soluzione A</summary> 
	
```sql
SELECT * FROM `phyloAccCNEE`
WHERE `CNEEid` IN (SELECT `CNEEid` FROM `CNEEtable`
                    WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
                                        WHERE `Intensified` = 1
                                        AND `CSUBST` = 1));
```
</details>

<details>
<summary>Soluzione B</summary> 
	
```sql
SELECT * FROM `phyloAccCNEE`
WHERE `CNEEid` IN (SELECT SUBSTRING_INDEX(`CNEEid`, '.', 1) AS `id`
			FROM `CNEEtable`
			WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
						WHERE `Intensified` = 1));
						AND `CSUBST` = 1));
```
</details>

E ad esempio contare la frequenza per i modelli `M0`, `M1` e `M2` per i miei risultati sui geni. 
<details>
<summary>Soluzione</summary> 
	
```sql
SELECT p.Model, count(*) FROM `phyloAccCNEE` p
WHERE `CNEEid` IN (SELECT SUBSTRING_INDEX(`CNEEid`, '.', 1) AS `id`
					FROM `CNEEtable`
					WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
							WHERE `Intensified` = 1
							AND `CSUBST` = 1))
GROUP BY p.`Model`;
```
</details>

### - L'uso `SELECT` per creare nuove tabelle organizzare diversamente i dati.
Mettiamo il caso che io voglia estrarre ed organizzare i dati che ho strutturato in certo modo in un altro che io reputo piu' snello, magari per una normalizzazione.
Posso utilizzare il comando `SELECT` per creare una nuova tabella.

La tabella `phyloAccCNEE` ad esempio contiene dati ridondanti poco utili in questo momento. Nella tabella sono elencati tutti i valori per i rate di accelerazione e conservazione per i tre modelli ma solo uno di questi set di valori sono corrispondenti al *best fit*.
Quello che possiamo decidere di fare e' creare una nuova tabella in cui sono elencati solo i valori per il *best fit model*.

Per prima cosa pensiamo a creare la nostra `SELECT` selezionando i campi appropriati per il `best model` `M1`:

<details>
<summary>Soluzione</summary> 

```sql
SELECT `CNEEid`,`GeneName`,`Model`,`logbf1`,`logbf2`,`logbf3`,`conservedRateM1` AS `conservedRate`,`accelRateM1` AS `accelRate`,`NumAccelM1` AS `NumAccel`
FROM `phyloAccCNEE`
WHERE `Model` = 'M1';
```
</details>

Ora usiamo la queri per creare e inserire i dati all'interno di una nuova tabella `phyloAccCNEE_New`

<details>
<summary>Soluzione</summary> 

```sql
CREATE TABLE `phyloAccCNEE_New` AS
SELECT `CNEEid`,`GeneName`,`Model`,`logbf1`,`logbf2`,`logbf3`,`conservedRateM1` AS `conservedRate`,`accelRateM1` AS `accelRate`,`NumAccelM1` AS `NumAccel`
FROM `phyloAccCNEE`
WHERE `Model` = 'M1';
```
</details>

Ecco una nuova tabella con i dati relativi ai CNEE con best model `M1`

Facciamo lo stesso per `M2`, ma invece di creare una nuova tabella inseriamo i nuovi dati delle tabella appena creata:

<details>
<summary>Soluzione</summary> 

```sql
INSERT INTO `phyloAccCNEE_New`
SELECT `CNEEid`,`GeneName`,`Model`,`logbf1`,`logbf2`,`logbf3`,`conservedRateM2` AS `conservedRate`,`accelRateM2` AS `accelRate`,`NumAccelM2` AS `NumAccel`
FROM `phyloAccCNEE`
WHERE `Model` = 'M2';
```
</details>

E ora per `M0`, ma facendo attenzione al campo `NumAccelM*`, esiste relativo a `M0`? Con cosa possiamo sostiturilo?

<details>
<summary>Soluzione</summary> 

```sql
INSERT INTO `phyloAccCNEE_New`
SELECT `CNEEid`,`GeneName`,`Model`,`logbf1`,`logbf2`,`logbf3`,`conservedRateM2` AS `conservedRate`,`accelRateM2` AS `accelRate`, 0 AS `NumAccel`
FROM `phyloAccCNEE`
WHERE `Model` = 'M0';
```
</details>




Voglio creare una nuova tabella da valori ridondanti di un'altra e sostituire questi valori con nuovi id creati al momento della creazione della nuova tabella

CREATE TABLE new_table (
    new_id INT AUTO_INCREMENT PRIMARY KEY,
    distinct_value VARCHAR(255) -- Adjust the data type as per your needs
);

INSERT INTO new_table (distinct_value)
SELECT DISTINCT redundant_value_column
FROM your_original_table;

Update sulla tabella originale

UPDATE your_original_table AS t
INNER JOIN new_table AS nt ON t.redundant_value_column = nt.distinct_value
SET t.new_id_column = nt.new_id;

Questo per usare una nuovo campo

ALTER TABLE your_original_table
ADD COLUMN new_id_column INT; -- Adjust the data type as per your needs

UPDATE your_original_table AS t
INNER JOIN new_table AS nt ON t.redundant_value_column = nt.distinct_value
SET t.new_id_column = nt.new_id;
