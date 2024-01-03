# Operatori Aritmetici SQL: Esempi
In questa lettura, imparerai di più sugli operatori aritmetici che possono essere utilizzati con SQL. Hai appreso gli operatori aritmetici in SQL che vengono utilizzati per eseguire operazioni matematiche di base come l'addizione, la sottrazione, la moltiplicazione e la divisione. Hai esplorato anche l'operatore del modulo, che restituisce il resto di una divisione matematica. L'obiettivo principale di questa lettura è presentare alcuni esempi in più di come gli operatori aritmetici possono essere utilizzati, inclusi scenari più avanzati.

Operatori Aritmetici
Gli operatori aritmetici sono utili quando si desidera eseguire operazioni matematiche sui dati nelle tabelle mentre li si recupera scrivendo le query di SELECT SQL. In SQL, gli operatori aritmetici vengono utilizzati per eseguire operazioni matematiche sui dati. Più specificamente, vengono utilizzati con dati numerici memorizzati nelle tabelle del database.

Gli operatori aritmetici possono essere utilizzati nella clausola SELECT così come nella clausola WHERE in una dichiarazione di SELECT SQL. Quando un operatore viene utilizzato nella clausola WHERE, è destinato a eseguire le operazioni solo su righe specifiche. Questo perché la clausola WHERE in SQL viene utilizzata per filtrare i dati su cui sta lavorando una particolare dichiarazione SQL.

Tutti gli operatori aritmetici vengono utilizzati su operandi numerici per eseguire:

- Addizione
- Sottrazione
- Moltiplicazione
- Divisione
- Modulo

Utilizzo dell'operatore di addizione
L'operatore di addizione SQL esegue l'operazione matematica di addizione su dati numerici all'interno delle colonne di una tabella. Ad esempio, se si desidera aggiungere i valori di due istanze di dati numerici da due colonne separate nella tabella, è necessario specificare le due colonne come primo e secondo operando. La sintassi è la seguente:

```sql
SELECT nome_colonna1 + nome_colonna2 FROM nome_tabella;
```

Esempio: Tabella dipendenti di un database aziendale

| ID_dipendente | Nome_dipendente | Stipendio | Indennità |
|---------------|------------------|-----------|-----------|
| 1             | Alex             | 25000     | 1000      |
| 2             | John             | 55000     | 1000      |
| 3             | James            | 52000     | 1000      |
| 4             | Sam              | 30000     | 1000      |

Se si desidera conoscere gli stipendi totali di tutti i dipendenti con lo stipendio base e l'indennità aggiunta, è possibile utilizzare l'operatore di addizione. La sintassi SQL per l'operatore di addizione è la seguente:

```sql
SELECT stipendio + indennità FROM dipendente;
```

Nell'esempio sopra, le colonne stipendio e indennità sono i due operandi. L'operatore di addizione viene utilizzato per aggiungere i valori di queste due colonne insieme. L'output sarà il seguente:

| Stipendio + Indennità |
|-----------------------|
| 26000                 |
| 56000                 |
| 53000                 |
| 31000                 |

Esempio dell'operatore di addizione nella clausola WHERE utilizzando i dati nella tabella dipendenti:

| ID_dipendente | Nome_dipendente | Stipendio | Indennità |
|---------------|------------------|-----------|-----------|
| 1             | Alex             | 24000     | 1000      |
| 2             | John             | 55000     | 1000      |
| 3             | James            | 52000     | 1000      |
| 4             | Sam              | 24000     | 1000      |

Supponiamo di voler recuperare gli stipendi dei dipendenti il cui stipendio totale è 25000. Ecco come è possibile utilizzare l'operatore di addizione in SQL:

```sql
SELECT *
FROM dipendente
WHERE stipendio + indennità = 25000;
```

La dichiarazione SQL filtra i record di tutti i dipendenti il cui salario totale (salario più indennità) è 25000.

L'output mostra i record di due dipendenti con ID 1 e 4 come nella tabella sottostante.

| employee_ID | employee_name | salary | allowance |
|-------------|---------------|--------|-----------|
| 1           | Alex          | 24000  | 1000      |
| 4           | Sam           | 24000  | 1000      |

Utilizzando l'operatore di sottrazione
L'operatore di sottrazione SQL esegue la sottrazione matematica su dati numerici all'interno delle colonne di una tabella del database. Se si desidera sottrarre i valori di una colonna numerica dai valori di un'altra colonna numerica, è necessario specificare entrambe le colonne come primo e secondo operando insieme all'operatore di sottrazione. La sintassi è la seguente:

```sql
SELECT column_name1 - column_name2 FROM table_name;
```

Ecco un esempio. Ecco di nuovo la tabella dei dipendenti, ma questa volta con una colonna "Tax" e diverse istanze di nuovi dati.

| employee_ID | employee_name | salary | allowance | tax |
|-------------|---------------|--------|-----------|-----|
| 1           | Alex          | 24000  | 1000      | 1000|
| 2           | John          | 55000  | 1000      | 2000|
| 3           | James         | 52000  | 1000      | 2000|
| 4           | Sam           | 24000  | 1000      | 1000|

Supponiamo che si desideri recuperare i salari dei dipendenti dopo aver dedotto le tasse. Questa è la sintassi SQL che è possibile utilizzare con l'operatore di sottrazione per ottenere questi risultati.

```sql
SELECT salary - tax FROM employee;
```

Qui, le colonne salary e tax sono gli operandi, e l'operatore di sottrazione viene applicato a loro. I valori nella colonna tax vengono sottratti dai valori nella colonna salary. L'output è il seguente:

| salary - tax |
|--------------|
| 23000        |
| 53000        |
| 50000        |
| 23000        |
| 23000        |

Ecco un esempio di utilizzo dell'operatore di sottrazione nella clausola WHERE. Considera la stessa tabella dei dipendenti e i dati. Se desideri scoprire chi guadagna uno stipendio di 50000 dopo la detrazione fiscale, questa è la query SQL che puoi scrivere:

```sql
SELECT * 
FROM employee 
WHERE salary - tax = 50000;
```

Qui, stai filtrando i dipendenti che ricevono uno stipendio di 50000 dopo le tasse nella clausola WHERE. Questo è il risultato che otterresti:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|---------------|--------|-----------|-----|
| 3           | James         | 52000  | 1000      | 2000|

Usando l'operatore di moltiplicazione
L'operatore di moltiplicazione SQL esegue l'operazione di moltiplicazione matematica su colonne di dati numerici in una tabella del database. Se si desidera moltiplicare i valori di due colonne numeriche, è necessario specificare entrambe le colonne come primo e secondo operando con l'operatore di moltiplicazione tra di esse.

Supponiamo che nella tabella dei dipendenti si desideri generare gli importi delle tasse per ciascun dipendente se questi importi vengono raddoppiati.

Si scriverebbe una dichiarazione SQL SELECT come questa.

```sql
SELECT tax * 2 FROM employee;
```

Qui, stai raddoppiando le tasse per tutti i dipendenti moltiplicando il valore della colonna tax per 2.

Il risultato sarebbe il seguente:

| tax * 2 |
|---------|
| 2000    |
| 4000    |
| 4000    |
| 2000    |

Ora rivediamo un esempio di come utilizzare l'operatore di moltiplicazione nella clausola WHERE. Supponiamo che tu voglia sapere chi deve pagare un importo di tasse pari a 4000, dopo aver raddoppiato il valore attuale delle tasse.

La query SELECT restituisce il risultato desiderato, utilizzando l'operatore di moltiplicazione nella clausola WHERE.

```sql
SELECT * 
FROM employee 
WHERE tax * 2 = 4000;
```

Qui, la clausola WHERE filtra i record dei dipendenti. Mostra chi pagherà un importo di tasse pari a 4000 dopo che l'importo attuale delle tasse è raddoppiato. Il risultato è il seguente:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|---------------|--------|-----------|-----|
| 2           | John          | 55000  | 1000      | 2000|
| 3           | James         | 52000  | 1000      | 2000|

Utilizzando l'operatore di divisione
L'operatore di divisione divide i valori numerici di una colonna per i valori numerici di un'altra colonna. La sintassi per l'utilizzo dell'operatore di divisione è la seguente:

```sql
SELECT column_name1 / column_name2 FROM table_name;
```

I dati nella tabella dei dipendenti sono i seguenti:

| employee_id | employee_name | salary | allowance | tax |
|-------------|---------------|--------|-----------|-----|
| 1           | alex          | 24000  | 1000      | 1000|
| 2           | John          | 55000  | 3000      | 2000|
| 3           | James         | 52000  | 3000      | 2000|
| 4           | Sam           | 24000  | 1000      | 1000|

In questo prossimo esempio, diciamo che si vuole scoprire la percentuale di indennità che ogni dipendente riceve, utilizzando il salario e l'importo dell'indennità.

È possibile scrivere una dichiarazione SQL SELECT con l'operatore di divisione come segue:

```sql
SELECT allowance / salary * 100 FROM employee;
```

Qui, sia l'operatore di divisione che quello di moltiplicazione vengono utilizzati insieme per dividere l'indennità per il salario e moltiplicare il risultato per 100 per trovare la percentuale di indennità.

Il risultato è il seguente:

| allowance / salary * 100 |
|--------------------------|
| 4.1667                   |
| 5.4545                   |
| 5.7692                   |
| 4.1667                   |

Come gli altri operatori aritmetici, questo può essere utilizzato anche nella clausola WHERE di una dichiarazione SELECT. Diciamo che si vuole scoprire quali dipendenti ricevono un'indennità di almeno il 5%.

Ecco come l'operazione di divisione è utilizzata nella clausola WHERE per ottenere questo:

```sql
SELECT *  
FROM employee 
WHERE allowance / salary * 100 >= 5;
```

Sia l'operatore di divisione che quello di moltiplicazione vengono utilizzati nella clausola WHERE per filtrare i dipendenti che ricevono un'indennità di almeno il 5% del loro salario. L'output è il seguente:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|---------------|--------|-----------|-----|
| 2           | John          | 55000  | 3000      | 2000|
| 3           | James         | 52000  | 3000      | 2000|

Utilizzando l'operatore di modulo
L'operatore di modulo (%) si comporta come ci si aspetta in SQL, restituendo il resto quando i valori numerici di una colonna vengono divisi per i valori numerici di un'altra colonna. La sintassi è la seguente:

```sql
SELECT column_name1 % column_name2 FROM table_name;
```

In questo esempio, stai lavorando con i seguenti dati nella tabella dei dipendenti.

| employee_ID | employee_name | salary | hours | allowance | tax |
|-------------|---------------|--------|-------|-----------|-----|
| 1           | alex          | 24000  | 10    | 1000      | 1000|
| 2           | John          | 55000  | 11    | 3000      | 2000|
| 3           | James         | 52000  | 7     | 3000      | 2000|
| 4           | Sam           | 24000  | 11    | 1000      | 1000|

Vuoi scoprire se il numero di ore lavorate da ciascun dipendente è un numero pari. Puoi scoprirlo emettendo la seguente dichiarazione SQL:

```sql
SELECT hours % 2 FROM employee;
```

Qui, l'operatore di modulo è applicato alla colonna delle ore per vedere se c'è un resto quando è diviso per 2. Se il resto è zero, ciò significa che il numero di ore lavorate da quel dipendente è un numero pari. Se il resto è maggiore di 0, ciò significa che il numero di ore lavorate da quel dipendente è un numero dispari.

In questo caso, l'output è il seguente:

| hours % 2 |
|-----------|
| 0         |
| 1         |
| 1         |
| 1         |

Come l'output mostra, solo il dipendente 1 ha lavorato per un numero pari di ore. Il resto dei dipendenti ha lavorato per un numero dispari di ore. Se vuoi filtrare solo i dipendenti che hanno lavorato per un numero pari di ore, usa la seguente dichiarazione SELECT che utilizza l'operatore di modulo nella clausola WHERE:

```sql
SELECT * 
FROM employee 
WHERE hours % 2 = 0;
```

Il risultato di questa dichiarazione SQL mostra il record del dipendente con ID 1 come mostrato nella seguente tabella:

| employee_ID | employee_name | salary | hours | allowance | tax |
|-------------|---------------|--------|-------|-----------|-----|
| 1           | alex          | 24000  | 10    | 1000      | 1000|

In questa lettura, hai appreso di più sull'uso degli operatori aritmetici in SQL, inclusi l'addizione, la sottrazione, la moltiplicazione, la divisione e il modulo. Gli esempi forniti dovrebbero aiutarti a capire come questi operatori possono essere utilizzati nelle clausole SELECT e WHERE.
