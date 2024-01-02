In questa lettura imparerai di più sugli operatori di confronto SQL. Hai già appreso sugli operatori di confronto in SQL che vengono utilizzati per formare condizioni per filtrare i dati. Gli operatori di confronto sono utili quando vuoi scrivere condizioni nella clausola WHERE di una dichiarazione SELECT per filtrare i record da una tabella. L'obiettivo principale di questa lettura è presentare ulteriori esempi di come questi operatori di confronto possono essere utilizzati, insieme a scenari più avanzati.

**Rinfrescando gli operatori di confronto SQL:**

Come hai già appreso, tutti gli operatori di confronto utilizzati con altri linguaggi di programmazione funzionano anche con SQL.

Questi operatori di confronto sono:

| Operatore | Cosa fa                |
|-----------|------------------------|
| =         | Controlla l'uguaglianza |
| <> o !=   | Controlla la disuguaglianza |
| >         | Controlla se qualcosa è maggiore di |
| >=        | Controlla se qualcosa è maggiore o uguale a |
| <         | Controlla se qualcosa è minore di |
| <=        | Controlla se qualcosa è minore o uguale a |

Hai visto esempi di come utilizzare questi operatori di confronto, ma esamineremo alcuni scenari aggiuntivi qui.

**Utilizzo dell'operatore di uguaglianza:**

Puoi utilizzare l'operatore `=` per testare l'uguaglianza in una query. Confronta l'uguaglianza di due espressioni. L'operatore di uguaglianza è utilizzato nella condizione della clausola WHERE di una dichiarazione SELECT.

Consideriamo questo esempio di tabella e i suoi dati:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
1           | alex          | 24000  | 10    | 1000      | 1000
2           | John          | 55000  | 11    | 3000      | 2000
3           | James         | 52000  | 7     | 3000      | 2000
4           | Sam           | 24000  | 11    | 1000      | 1000
```

Se desideri recuperare i dati del dipendente con valore ID pari a 1, puoi utilizzare questa dichiarazione SELECT:

```sql
SELECT * FROM employee WHERE employee_id = 1;
```

Questa dichiarazione filtra il record del dipendente il cui valore nella colonna ID è uguale a 1. Il risultato è il seguente:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
1           | Alex          | 24000  | 10    | 1000      | 1000
```

Questo è stato un esempio di utilizzo dell'operatore di uguaglianza con una colonna di tipo dati numerico. Ora esamineremo un esempio di utilizzo dell'operatore di uguaglianza con una colonna di tipo dati basata su testo, ad esempio `employee_name`.

In questo esempio, devi recuperare i dati del dipendente il cui nome è James. Puoi utilizzare l'operatore di uguaglianza nella condizione della clausola WHERE:

```sql
SELECT * FROM employee WHERE employee_name = 'James';
```

L'importante da notare qui è che il valore o il letterale, in questo caso James, è racchiuso tra virgolette singole.

**Utilizzo dell'operatore di disuguaglianza:**

L'operatore di disuguaglianza fa l'opposto di ciò che fa l'operatore di uguaglianza. Confronta due espressioni non nulle e restituisce true se il valore dell'espressione a sinistra non è uguale a quella a destra. In caso contrario, restituisce false.

Ci sono due modi in SQL in cui può essere utilizzato, `<>` o `!=`, e entrambi i metodi producono lo stesso risultato.

Ad esempio, diciamo che vuoi determinare quale dipendente riceve uno stipendio che non equivale a 24000. Puoi utilizzare la seguente dichiarazione SQL:

```sql
SELECT * FROM employee WHERE salary <> 24000;
```

L'espressione qui è `salary <> 24000`. Questa espressione verifica la disuguaglianza del valore dato nella colonna salary e filtra solo quei record. Il risultato qui è il seguente:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
2           | John          | 55000  | 11    | 3000      | 2000
3           | James         | 52000  | 7     | 3000      | 2000
```

Puoi eseguire la stessa query semplicemente sostituendo `<>` con `!=`. Dovrebbe comportarsi allo stesso modo e dare lo stesso risultato.

**Utilizzo dell'operatore maggiore di:**

Questo operatore di confronto confronta due espressioni non nulle e restituisce true se l'operando di sinistra è maggiore dell'operando di destra. In caso contrario, il risultato è false.

Supponiamo che tu voglia scoprire quali dipendenti guadagnano uno stipendio superiore a 50000. Scriveresti la query SQL SELECT come segue utilizzando l'operatore di confronto maggiore di:

```sql
SELECT * FROM employee WHERE salary > 50000;
```

L'espressione qui è `salary > 50000`. Questa espressione verifica se il valore nella colonna salary è maggiore di 50000. In tal caso, include quei dipendenti nei risultati.

Di conseguenza, il risultato è:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
2           | John          | 55000  | 11    | 3000      | 2000
3           | James         | 52000  | 7     | 3000      | 2000
```

**Utilizzo dell'operatore maggiore o uguale a:**

L'operatore maggiore o uguale a (`>=`) confronta due espressioni non nulle. Il risultato è true se l'espressione di sinistra è un valore maggiore del valore dell'espressione di destra. Quest

a volta diciamo che vuoi vedere chi paga una tassa di almeno 2000. Questa è la query che darà il risultato desiderato utilizzando l'operatore maggiore o uguale a:

```sql
SELECT * FROM employee WHERE tax >= 1000;
```

L'espressione qui è `tax >= 1000` e verifica se il valore della colonna tax è maggiore o uguale a 1000. Se trova record corrispondenti, questi vengono aggiunti al set di risultati.

Il risultato in questo caso è il seguente:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
1           | Alex          | 24000  | 10    | 1000      | 1000
2           | John          | 55000  | 11    | 3000      | 2000
3           | James         | 52000  | 7     | 3000      | 2000
4           | Sam           | 24000  | 11    | 1000      | 1000
```

Tutti e 4 i record sono inclusi nel risultato perché la query SQL confronta la colonna tax e seleziona le righe che hanno un valore di 1000 o più.

**Utilizzo dell'operatore minore di:**

L'operatore `<` in SQL può essere utilizzato per testare un'espressione che è inferiore a. Vale a dire, confronta due espressioni non nulle. Il risultato è true se l'operando di sinistra è valutato a un valore inferiore al valore dell'operando di destra. In caso contrario, il risultato è false.

Ad esempio, diciamo che vuoi determinare quali dipendenti ottengono un'indennità inferiore a 2500. La seguente query SQL può essere utilizzata per recuperare il risultato:

```sql
SELECT * FROM employee WHERE allowance < 2500;
```

L'espressione `allowance < 2500` verifica i valori della colonna allowance per determinare quali hanno un valore inferiore a 2500.

Il risultato è il seguente:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
1           | alex          | 24000  | 10    | 1000      | 1000
4           | Sam           | 24000  | 11    | 1000      | 1000
```

**Utilizzo dell'operatore minore o uguale a:**

In SQL, l'operatore `<=` verifica un'espressione minore o uguale a. Vale a dire, confronta due espressioni non nulle e restituisce true se l'espressione di sinistra ha un valore minore o uguale al valore dell'espressione di destra. In caso contrario, restituisce true.

Lo useresti, ad esempio, se vuoi determinare quali dipendenti hanno lavorato per meno o uguale a 10 ore. La seguente sintassi è un esempio di come utilizzaresti l'operatore minore o uguale nella clausola WHERE della dichiarazione SELECT:

```sql
SELECT * FROM employee WHERE hours <= 10;
```

L'espressione `hours <= 10` verifica i valori della colonna hours per vedere se ci sono record con un valore di 10 o meno. Se ne trova uno, lo aggiunge al risultato.

Di conseguenza, il risultato di questa query è il seguente:

```sql
employee_ID | employee_name | salary | hours | allowance | tax
------------|---------------|--------|-------|-----------|-----
1           | Alex          | 24000  | 10    | 1000      | 1000
3           | James         | 52000  | 7     | 3000      | 2000
```

In questa lettura hai appreso sugli operatori di confronto SQL e ti sei familiarizzato di più con come applicarli in diversi scenari.
