# Chapter 3

# Table Of Contents

> 1.  [database planning](#database-planning)
> 1.  [Current Settings and Parameter Change](#current-settings-and-parameter-change)

## database planning

> [!NOTE]
>
> 1.  Database Design - plan the DB
> 1.  Calculate the initial DB sizing
> 1.  Transaction analysis - how will we access the DB
> 1.  Look at the most frequently accessed path
> 1.  what are the requirements for response time
> 1.  hardware config
> 1.  Initial performance thoughts --> will all of data fit into RAM.
> 1.  Choose **OS** & **FileSystem** type
> 1.  How do we partition the disks
> 1.  Localization Path
> 1.  Decide the server **encoding**, local and **timezone**.
> 1.  Access and Security plan.
> 1.  Identify Client system and specify required drivers
> 1.  Create roles according to a plan for access control
> 1.  Specifying `pg_hba.conf` (**hba -> host based access**)
> 1.  Maintenance Plan
> 1.  Availability Plan (HA)
> 1.  Plan for Replication type (HA)
> 1.  Checkpoint - Timeout (**Backup and Restore**)
> 1.  Plan BKP mechanism

## Current Settings and Parameter Change

> To Check Postgres Settings :
>
> ```sql
> select * from pg_settings;
> ```

> Expand Display Settings :
>
> `psql=#`
>
> ```sql
>
> \x
> ```

> To Check Postgres Settings (Another way):
>
> ```sql
> show config_file;
> show hba_file;
> show ident_file;
> ```

> [!TIP]
> There are 200+ parameters
>
> ```sql
> SELECT name, setting FROM pg_settings WHERE source!='default' AND source!='override' order by 2,1;
> ```
>
> Many of the parameters can be changed while the server is still running. After changing the required parameters, we issue a reload operation to the server, forcing Postgres to reread the **`postgresql.conf`**
>
> ```sh
> pg_ctl reload
> ```
>
> Some Param needs restart
>
> ```sh
> pg_ctl restart
> ```

> [!TIP]
> We can update parameters using **`QUERY`** as well
>
>Example:
>```sql
>ALTER SYSTEM SET SHARED_BUFFERS='1GB'
>```
>Thi will not directly modify the `postgresql.conf` rather it will create a new file called - **`postgresql.auto.conf`**.
>
>---
>
>Setting Parameters for Particular users / groups
>
>CASES:
>```sql
>ALTER DATABASE <DB> SET config_param = <value>
>```
>
>```sql
>ALTER ROLE <USER> SET config_param = <value>
>```
>```sql
>ALTER ROLE <USER> in DATABASE <DB> SET config_param = <value>
>```
>
>---