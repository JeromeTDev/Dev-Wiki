|Begriff|Bedeutung|
|---|---|
|Database|Container für Tabellen|
|Relation|Tabelle|
|Row|Datensatz|
|Column|Feld|
### ENTITY CONTEXT

- n8n: workflow automation platform
- PostgreSQL: relational database system

---

### 1. IN POSTGRES CONTAINER REIN

```bash
docker exec -it n8n-postgres psql -U n8n
```

---

###  2. PSQL NAVIGATION

```sql
\l        -- alle Datenbanken anzeigen
\c dbname -- in Datenbank wechseln
\dt       -- Tabellen anzeigen
\d table  -- Tabellenstruktur anzeigen
\q        -- psql verlassen
```

Pager Navigation:

```text
q        = quit
j / ↓    = runter
k / ↑    = hoch
space    = Seite weiter
d        = halbe Seite runter
u        = halbe Seite hoch
```

---

### 3. DATENBANKEN

```sql
CREATE DATABASE mydb;
DROP DATABASE mydb;

\l

\c mydb
```

---

### 4. TABELLEN

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  created_at TIMESTAMP DEFAULT now()
);

\dt
\d users

DROP TABLE users;
```

---

### 5. DATEN EINFÜGEN

```sql
INSERT INTO users (name) VALUES ('woodz');
```
### 6. DATEN LESEN

```sql
SELECT * FROM users;
SELECT * FROM users WHERE id = 1;
```
### 7. DATEN ÄNDERN

```sql
UPDATE users
SET name = 'newname'
WHERE id = 1;
```
### 8. DATEN LÖSCHEN

```sql
DELETE FROM users WHERE id = 1;
```
### 9. SYSTEM INFORMATIONEN

```sql
SELECT version();
SELECT current_database();
SELECT current_user;
```

---
5
### 10. DOCKER + POSTGRES BASICS

```bash
docker compose up -d
docker compose down

docker compose logs -f postgres
docker compose logs -f n8n

docker ps
docker ps -a
```

---

### 11. IN CONTAINER ARBEITEN

```bash
docker exec -it n8n-postgres psql -U n8n
```

```sql
\c bdc
\dt
```

---

### 12. BACKUP / RESTORE

Backup:

```bash
docker exec -t n8n-postgres pg_dump -U n8n n8n > backup.sql
```

Restore:

```bash
cat backup.sql | docker exec -i n8n-postgres psql -U n8n n8n
```

---

### 13. USER & RECHTE

```sql
CREATE USER appuser WITH PASSWORD 'secret';

GRANT ALL PRIVILEGES ON DATABASE mydb TO appuser;
```

---

### 14. N8N TABELLEN (BEISPIEL)

```sql
\dt

SELECT * FROM workflow_entity;
SELECT * FROM execution_entity;
SELECT * FROM credentials_entity;
```

---

### 15. HÄUFIGE FEHLER

- /l statt \l
- ls in psql
- Container export statt pg_dump
- keine Volumes nutzen
- 
- alles in eine DB werfen

---

### 16. MINI WORKFLOW TEST

```sql
\c bdc

CREATE TABLE test (
  id SERIAL PRIMARY KEY,
  name TEXT
);

INSERT INTO test (name) VALUES ('hello');

SELECT * FROM test;
```

---

### MERKSATZ

- Docker startet Postgres
- Postgres verwaltet Datenbanken
- SQL arbeitet in einer DB
- Tabellen sind dein Datenmodell