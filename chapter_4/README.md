# Chapter 4 : Server Control

# Table of Contents

1. [setting the database server manually](#setting-the-database-server-manually)
1. [stopping the server safely and quickly](#stopping-the-server-safely-and-quickly)
1. [stopping the server in emergency](#stopping-the-server-in-emergency)
1. [reloading the server config files](#reloading-the-server-config-files)
1. [restarting the server in quickly](#restarting-the-server-in-quickly)
1. [preventing new connections](#preventing-new-connections)
1. [restricting users](#restricting-users)
1. [deciding on a design for multienancy](#deciding-on-a-design-for-multienancy)
1. [using multiple schemas](#using-multiple-schemas)
1. [private DB for user](#private-db-for-user)
1. [running multiple servers on one system](#running-multiple-servers-on-one-system)
1. [setting up a connection pool](#setting-up-a-connection-pool)
1. [accessing multiple servers using same host and port](#accessing-multiple-servers-using-same-host-and-port)

---

## setting the database server manually

> Create DIRECTORY
>
> ```sh
> mkdir MYDB
> realpath MYDB/
> ```
>
> Change directory to your DB PATH
>
> ```sh
> cd MYDB
> ```
>
> ```sh
> pwd | xargs -I {} sh -c 'pg_ctl initdb -D {}'
> ```
>
> or
>
> ```sh
> export PGDATA=</real/path/to/MYDB>
> pg_ctl initdb -D $PGDATA
> ```

> Edit `postgresql.conf`
>
> 1. set **port no**
> 1. set **hostip**
> 1. set **work_mem** from `4MB` to some `1GB`
> 1. `save`

> Edit **`pg_hba.conf`**
>
> add below line to allow all users for all database in this hosted postgres instance.
>
> ```
> host    all     all      0.0.0.0/0     md5
> ```
>
> `saved...`

> START DB
>
> ```sh
> pg_ctl -D <real/path/to/MYDB> -l <pgserver_out_[port]>.log start
> ```

## stopping the server safely and quickly

>[!NOTE]
> there are 3 modes :
> 1. smart
> 1. fast
> 1. immediate
>
>```bash
>pg_ctl -D <real/path/to/MYDB> -m [smart|fast|immediate] <start/stop/restart>
>```

>[!TIP]
>
>One Best Practice to restart PGSQL server is to  create CHECKPOINT
>
>```sql
>psql -c "CHECKPOINT"
>```

## preventing new connections

>---
>
>```sql
>ALTER DATABASE <DBNAME> connection LIMIT 0;
>```
> it will be required before dropping the DATABASE also if connections are there.
>
>---
>For Specific user :
>```sql
>ALTER USER <USER_NAME> CONNECTION LIMIT 0;
>```
>---

## Restricting users

>[!IMPORTANT]
>
>TO SET DEFAULT SCHEMA for a Particular user
>
>```sql
>ALTER ROLE <user> SET search_path='SCHEMA_NAME';
>```

>```sql
>REVOKE ALL ON SCHEMA <SCHEMANAME> FROM public;
>```
>
>```sql
>GRANT ALL ON SCHEMA <SCHEMANAME> TO <user>;
>```

## private DB for user

>**Script:**
>```sql
>BEGIN
>CREATE user fred;
>CREATE DATABASE FRED_DB owner=fred;
>REVOKE CONNECT ON DATABASE FRED_DB FROM public;
>GRANT CONNECT ON DATABASE FRED_DB TO fred;
>commit;
>```

## running multiple servers on one system

> **similar to point 1**.