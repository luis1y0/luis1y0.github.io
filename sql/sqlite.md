# SQLite

## Comandos basicos

Se ejecuta `sqlite3 <dbfile>` para abrir un archivo o crearlo, en caso de no usar
un archivo, se accede a la shell generando una base de datos temporal en memoria
con las siguientes opciones:

```
.exit
.help
.schema [table]
.fullschema
.mode [ascii|box|csv|column|html|insert|json|line|list|markdown|quote|table]
.nullvalue "  --  NULL --"
.read <file.sql>
.databases
.tables
.indexes [table]
.excel
 -- skip para omitir primera fila --
.import --csv --skip 1 --schema db C:/work/somedata.csv table
 -- export --
sqlite> .headers on
sqlite> .mode csv
sqlite> .once c:/work/dataout.csv
sqlite> SELECT * FROM tab1;
sqlite> .system c:/work/dataout.csv
-- backup --
sqlite3 dbfile .dump > file.sql
```

## Tablas reservadas

 - sqlite_stat1
 - sqlite_schema

## Queries utiles

```sql
SELECT name FROM sqlite_schema WHERE type IN ('table','view') AND name NOT LIKE 'sqlite_%';
SELECT ROW_NUMBER() OVER (), * FROM job_history;
```
# PRAGMAS

```
PRAGMA foreign_key_list(job_history);
PRAGMA collation_list;
PRAGMA function_list;
PRAGMA full_column_names = TRUE;
```
