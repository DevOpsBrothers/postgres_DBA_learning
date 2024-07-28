# Chapter 2: Exploring DB

---

![My Skills](https://go-skill-icons.vercel.app/api/icons?i=postgres,linux,docker,bash,&perine=6)

---

> [!NOTE]
>
> Topics to be covered :
>
> 1. [Version of server](#version-of-server)
> 1. [Checking **Server Uptime**](#checking-server-uptime)
> 1. **DB Server** file structure
> 1. Locating _DB Server_ **MSG LOG**
> 1. Locating the DB system identifier
> 1. Locating **Databases** in **DB Server**
> 1. Listing **DBs**
> 1. Listing How many _tables_ are there
> 1. Checking **Disk Space usage** by a _table_
> 1. Which are the biggest _tables_
> 1. Listing How many rows are there in a _table_
> 1. Quickly estimating the no of rows in a _table_
> 1. Extensions in DB
> 1. **Check Object Dependency**

---

## Version of server

> [!TIP]
>
> ```sql
> select version();
> ```
>
> Output:
>
> ```bash
> postgres=# select version();
>                                                   version       ion
> ------------------------------------------------------------------------------------------------------------------------
>                                                       cc (Alpine
> PostgreSQL 16.3 on x86_64-pc-linux-musl, compiled by gcc (Alpine 13.2.1_git20240309) 13.2.1 20240309, 64-bit
> (1 row)
> ```
>
> Another way:
>
> ```bash
> cat $PGDATA/PG_VERSION
> ```
>
> ```bash
> psql --version
> ```

## Checking **Server Uptime**

> [!IMPORTANT]
> To check the up time of the server we can use below query
>
> ```sql
> select date_trunc('second', current_timestamp - pg_postmaster_start_time()) as uptime;
> ```
>
> output:
>
> ```bash
>  uptime
> ----------
> 00:57:14
> (1 row)
> ```
