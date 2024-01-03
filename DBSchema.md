

# Creazione di uno schema di database
In questa lettura, sarai guidato attraverso un esempio di creazione di uno schema di database semplice. Hai appreso il concetto di uno schema di database e cosa deve essere fatto per costruirlo. L'obiettivo principale di questa lettura è presentare un esempio più complesso di creazione di uno schema di database.

## **Schema di database**
La creazione di uno schema di database è il primo passo nel design del database. È essenziale, specialmente quando si tratta di database relazionali, poiché si desidera una struttura solida per il database prima di poter procedere. Uno schema di database è come un progetto di come appariranno e saranno memorizzati i dati in un database.

Uno schema è composto da oggetti noti come oggetti di schema. Gli oggetti di schema potrebbero essere cose come tabelle, colonne e relazioni, almeno. Tipi di dati, viste, stored procedure, chiavi primarie e chiavi esterne sono anche oggetti di schema.

Fondamentalmente, uno schema di database consiste in:

- tutti i dati importanti relativi a uno scenario specifico e le loro relazioni,
- chiavi univoche per tutte le voci e gli oggetti di database,
- e un nome e tipo di dati per ogni colonna in una tabella.

## **Costruzione di uno schema di database per uno scenario di prenotazione di un ristorante**
Nella costruzione di uno schema di database per un sistema di prenotazione di un ristorante, ci sono alcune cose da considerare. Si deve considerare che i clienti fanno prenotazioni per tavoli e quei tavoli hanno ordini associati. Un ordine avrà elementi del menu associati che appartengono a un menu. E gli ordini sono serviti da un cameriere.

**Lo schema di database logico**
Ora esaminiamo come costruire uno schema di database logico per questo scenario. In un esempio come questo, gli ingegneri di database disegnano generalmente un diagramma noto come ER-D (Entity Relationship Diagram).

Lo schema di database logico è composto da entità che diventano tabelle nella progettazione fisica del database. Ogni entità ha un insieme di attributi e uno di essi (a volte anche due) rende ogni istanza dell'entità, o riga di dati, unica. Questi attributi sono noti come chiave primaria. Questi attributi di chiave primaria sono presenti anche in altre tabelle a cui la tabella è correlata. In una tabella correlata, questa chiave è nota come chiave esterna.

Questo è lo schema logico o l'ER-D per lo scenario.
![image](https://github.com/francicco/SQLtutorials/assets/9006870/f1581616-a9af-4869-933e-cd37fe9a68a8)

## **Lo schema di database fisico**
Ora costruiamo lo schema di database fisico per questo scenario sulla base dello schema di database logico progettato nella sezione precedente. Il primo passo è creare il database del ristorante.

Per creare il database del ristorante, la sintassi SQL CREATE DATABASE è la seguente:

```sql
CREATE DATABASE restaurant;
```

Il passo successivo è creare le tabelle all'interno di questo database. Le tabelle e i loro campi o colonne che devono essere creati si trovano nello schema di database logico. Deve essere utilizzato un tipo di dati appropriato quando si definiscono le colonne delle tabelle. Ciò consente un'allocazione corretta della memoria durante la memorizzazione fisica dei dati.

Quindi, esaminiamo la sintassi per creare ciascuna tabella.

La prima tabella 'tbl' rappresenta un tavolo nel ristorante. Ha un ID univoco e una posizione, dove è posizionato nel ristorante. L'ID univoco è la chiave primaria di questa tabella.

```sql
CREATE TABLE tbl( 
    table_id INT, 
    location VARCHAR(255), 
    PRIMARY KEY (table_id) 
); 
```

La tabella successiva contiene dati sui camerieri che lavorano nel ristorante. Hanno un ID univoco, un nome, il numero di contatto e quale turno lavorano di solito. La chiave primaria della tabella è l'ID univoco assegnato al cameriere.

```sql
CREATE TABLE waiter( 
    waiter_id INT, 
    name VARCHAR(150), 
    contact_no VARCHAR(10), 
    shift VARCHAR(10),
    PRIMARY KEY (waiter_id) 
);
```

La sintassi seguente crea la tabella che memorizza i dati sugli ordini per ogni tavolo. Ha i campi order ID e table ID. Oltre a un campo date_time per catturare la data e l'ora dell'ordine e l'ID del cameriere che dovrebbe servire quel tavolo, per quell'ordine.

```sql
CREATE TABLE table_order( 
    order_id INT, 
    date_time DATETIME, 
    table_id INT, 
    waiter_id INT,
    PRIMARY KEY (order_id), 
    FOREIGN KEY (table_id) REFERENCES tbl(table_id), 
    FOREIGN KEY (waiter_id) REFERENCES waiter(waiter_id) 
);
```

Questa tabella memorizza i dati sui clienti. Ha un ID cliente, nome, numero NIC per memorizzare il numero della carta d'identità e i campi del numero di contatto. La chiave primaria è il campo ID cliente univoco.

```sql
CREATE TABLE customer( 
    customer_id INT, 
    name VARCHAR(100), 
    NIC_no VARCHAR(12), 
    contact_no VARCHAR(10),
    PRIMARY KEY (customer_id) 
);
```

La tabella di prenotazione associa un ordine a un cliente. Ha un ID univoco, data e ora, numero di ospiti o pax previsti, l'order_id, table_id e il customer_id. La sua chiave primaria è l'ID di prenotazione univoco. Questa tabella è collegata alle tabelle tbl, table_order e customer.

```sql
CREATE TABLE reservation( 
    reservation_id INT, 
    date_time DATETIME, 
    no_of_pax INT, 
    order_id INT, 
    table_id INT, 
    customer_id INT,
    PRIMARY KEY (reservation_id), 
    FOREIGN KEY (order_id) REFERENCES table_order(table_id), 
    FOREIGN KEY (table_id) REFERENCES tbl(table_id), 
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id) 
);
```

Questa tabella dei menu memorizza tutti i menu del ristorante. Ha un menu_id che è il campo univoco che contiene descrizioni del menu e la sua disponibilità.

```sql
CREATE TABLE menu( 
    menu_id INT, 
    description VARCHAR(255), 
    availability INT, 
    PRIMARY KEY (menu_id) 
);
```

Ogni menu può avere elementi del menu unici e questi elementi del menu sono memorizzati in menu_item, nel menu. Un elemento di menu ha anche descrizioni, prezzo e campi di disponibilità. Questa tabella si collega alla tabella menu.

```sql
CREATE TABLE menu_item( 
    menu_item_id INT, 
    description VARCHAR(255), 
    price FLOAT, 
    availability INT, 
    menu_id INT, 
    PRIMARY KEY (menu_item_id), 
    FOREIGN KEY (menu_id) REFERENCES menu(menu_id) 
);
```

Questa tabella finale cattura gli elementi del menu ordinati per un ordine specifico. Ha order_id, menu_item_id e la quantità ordinata. Ha una chiave primaria composta dalla combinazione di campi order_id e menu_item_id ed è collegata alle tabelle table_order e menu_item.

```sql
CREATE TABLE order_menu_item( 
    order_id INT, 
    menu_item_id INT, 
    quantity INT, 
    PRIMARY KEY (order_id,menu_item_id),
    FOREIGN KEY (order_id) REFERENCES table_order(order_id), 
    FOREIGN KEY (menu_item_id) REFERENCES menu_item(menu_item_id)
);
```

Queste dichiarazioni CREATE TABLE creano tutte le tabelle all'interno del database di prenotazione. La cosa importante da notare è come sono stabilite le relazioni tra le tabelle. Ogni tabella è definita con una chiave primaria, e questa a sua volta diventa la chiave esterna nella tabella correlata.

In conclusione, questo è come può essere creata una struttura di database di base o uno schema utilizzando sintassi SQL DDL (Data Definition Language).
