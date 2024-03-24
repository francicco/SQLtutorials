# SQL: Esplorazioni dei dati: uso di `SELECT`.

Per prima cosa possiamo iniziare con il chiederci quanti geni sono stati usati in questo esperimento?
Come possiamo estrarre questo dato?

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

Allo stesso modo, quali geni sono sotto intensificazione?
<details>
<summary>Soluzione</summary>
	
```sql
SELECT `OGid`
FROM `AnalisiOG`
WHERE `Intensified` = 1;
```
</details>

Quali e Quanti sotto intensificazione e convergenza?
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

Come possiamo raggruppare tutti i campi in una sola query?
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


Quanti CNEE ci sono per cromosoma?
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

Possiamo inoltre *rinominare* le cartelle:

```sql
SELECT AnalisiOG.*, CNEEtable.*
FROM AnalisiOG 
INNER JOIN CNEEtable 
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

<details>
<summary>Soluzione</summary> 
	
```sql
SELECT
	SUBSTRING_INDEX(`CNEEid`, '.', 1) AS `id`,
	SUBSTRING_INDEX(`CNEEid`, '.', 2) AS `gene`
FROM `CNEEtable`
WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
		WHERE `Intensified` = 1
		AND `CSUBST` = 1);
```
</details>

<details>
<summary>Soluzione</summary> 
	
```sql
SELECT * FROM `phyloAccCNEE`
WHERE `CNEEid` IN (SELECT `CNEEid` FROM `CNEEtable`
                    WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
                                        WHERE `Intensified` = 1
                                        AND `CSUBST` = 1));
```
</details>

<details>
<summary>Soluzione</summary> 
	
```sql
SELECT * FROM `phyloAccCNEE`
WHERE `CNEEid` IN (SELECT
			SUBSTRING_INDEX(`CNEEid`, '.', 1) AS `id`
			FROM `CNEEtable`
			WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
						WHERE `Intensified` = 1
						AND `CSUBST` = 1));
```
</details>

```sql
SELECT p.Model, count(*) FROM `phyloAccCNEE` p
WHERE `CNEEid` IN (SELECT SUBSTRING_INDEX(`CNEEid`, '.', 1) AS `id`
					FROM `CNEEtable`
					WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
							WHERE `Relaxed` = 1
							AND `CSUBST` = 1))
GROUP BY p.`Model`;
```

	
```sql
SELECT * FROM `CNEEtable`
WHERE `OGid` IN (SELECT `OGid` FROM `AnalisiOG`
		WHERE `Intensified` = 1
		AND `CSUBST` = 1);
```
