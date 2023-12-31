# MySQL Cheat Sheet

> Guida alle istruzioni SQL per interagire con un database MySQL

## Posizioni MySQL
* Mac             */usr/local/mysql/bin*
* Windows         */Program Files/MySQL/MySQL _version_/bin*
* Xampp           */xampp/mysql/bin*

## Aggiungi MySQL al tuo PATH

```bash
# Sessione corrente
export PATH=${PATH}:/usr/local/mysql/bin
# Permanentemente
echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile
```

Su Windows - https://www.qualitestgroup.com/resources/knowledge-center/how-to-guide/add-mysql-path-windows/

## Login

```bash
mysql -u root -p
```

## Visualizza Utenti

```sql
SELECT User, Host FROM mysql.user;
```

## Crea Utente

```sql
CREATE USER 'someuser'@'localhost' IDENTIFICATO DA 'somepassword';
```

## Conferisci Tutti i Privilegi su Tutti i Database

```sql
GRANT ALL PRIVILEGES ON * . * TO 'someuser'@'localhost';
FLUSH PRIVILEGES;
```

## Visualizza Concessioni

```sql
SHOW GRANTS FOR 'someuser'@'localhost';
```

## Rimuovi Concessioni

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'someuser'@'localhost';
```

## Elimina Utente

```sql
DROP USER 'someuser'@'localhost';
```

## Esci

```sql
exit;
```

## Visualizza Database

```sql
SHOW DATABASES
```

## Crea Database

```sql
CREATE DATABASE acme;
```

## Elimina Database

```sql
DROP DATABASE acme;
```

## Seleziona Database

```sql
USE acme;
```

## Crea Tabella

```sql
CREATE TABLE users(
id INT AUTO_INCREMENT,
   first_name VARCHAR(100),
   last_name VARCHAR(100),
   email VARCHAR(50),
   password VARCHAR(20),
   location VARCHAR(100),
   dept VARCHAR(100),
   is_admin TINYINT(1),
   register_date DATETIME,
   PRIMARY KEY(id)
);
```

## Elimina / Cancella Tabella

```sql
DROP TABLE tablename;
```

## Visualizza Tabelle

```sql
SHOW TABLES;
```

## Inserisci Riga / Record

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept, is_admin, register_date) values ('Brad', 'Traversy', 'brad@gmail.com', '123456','Massachusetts', 'development', 1, now());
```

## Inserisci Più Righe

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept,  is_admin, register_date) values ('Fred', 'Smith', 'fred@gmail.com', '123456', 'New York', 'design', 0, now()), ('Sara', 'Watson', 'sara@gmail.com', '123456', 'New York', 'design', 0, now()),('Will', 'Jackson', 'will@yahoo.com', '123456', 'Rhode Island', 'development', 1, now()),('Paula', 'Johnson', 'paula@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now()),('Tom', 'Spears', 'tom@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now());
```

## Seleziona

```sql
SELECT * FROM users;
SELECT first_name, last_name FROM users;
```

## Clausola Where

```sql
SELECT * FROM users WHERE location='Massachusetts';
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
SELECT * FROM users WHERE is_admin = 1;
SELECT * FROM users WHERE is_admin > 0;
```

## Cancella Riga

```sql
DELETE FROM users WHERE id = 6;
```

## Aggiorna Riga

```sql
UPDATE users SET email = 'freddy@gmail.com' WHERE id = 2;
```

## Aggiungi Nuova Colonna

```sql
ALTER TABLE users ADD age VARCHAR(3);
```

## Modifica Colonna

```sql
ALTER TABLE users MODIFY COLUMN age INT(3);
```

## Ordina Per (Ordina)

```sql
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

## Concatena Colonne

```sql
SELECT CONCAT(first_name, ' ', last_name) AS 'Name', dept FROM users;
```

## Seleziona Righe Distinte

```sql
SELECT DISTINCT location FROM users;
```

## Tra (Seleziona Intervallo)

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```

## Like (Ricerca)

```sql
SELECT * FROM users WHERE dept LIKE 'd%';
SELECT * FROM users WHERE dept LIKE 'dev%';
SELECT * FROM users WHERE dept LIKE '%t';
SELECT * FROM users WHERE dept LIKE '%e%';
```

## Not Like

```sql
SELECT * FROM users WHERE dept NOT LIKE 'd%';
```

## IN

```sql
SELECT * FROM users WHERE dept IN ('design', 'sales');
```

## Crea e Rimuovi Indice

```sql
CREATE INDEX LIndex Su users(location);
DROP INDEX LIndex Su users;
```

## Nuova Tabella Con Chiave Esterna (Posts)

```sql
CREATE TABLE posts(
id INT AUTO_INCREMENT,
   user_id INT,
   title VARCHAR(100),
   body TEXT,
   publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
   PRIMARY KEY(id),
   FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Aggiungi Dati alla Tabella Posts

```sql
INSERT INTO posts(user_id, title, body) VALUES (1, 'Post One', 'This is post one'),(3, 'Post Two', 'This is post two'),(1, 'Post Three', 'This is post three'),(2, 'Post Four', 'This is post four'),(5, 'Post Five', 'This is post five'),
