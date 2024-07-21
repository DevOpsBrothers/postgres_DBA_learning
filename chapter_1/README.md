# Chapter 1: First Steps

---

![My Skills](https://go-skill-icons.vercel.app/api/icons?i=postgres,linux,docker,bash,&perine=6)

---

> [!NOTE]
>
> 1. Topics :
> 1. Connecting PGSQL
> 1. **Enabling** Access for Network / Remote User
> 1. Use Of **GUI** tools
> 1. Use the **`psql`** query and scripting
> 1. Changing Password Securely
> 1. Avoiding Hardcoding Password
> 1. Using Connection **Service File**
> 1. Troubleshooting

---

## Connecting PGSQL

> [!IMPORTANT]
> If we don't provide preceding parameters , PostgreSQL looks for values set through **ENVIRONMENT** Variables, as follows:
>
> ```bash
> PGHOST=<hostval>
> PGPORT=5432 #(default)
> PGDATABASE=postgres #(default)
> PGUSER=postgres #(default)
> ```

> [!CAUTION]
> Not Recommended
> `PGPASSWORD=MyPassWord@123`

> [!TIP]
>
> ### Connection Details URI Format :
>
> ```link
> postgresql://user:password@HOST:PORT/DATABASE
> ```

> [!TIP]
>
> ### To Check current DB :
>
> ```sql
> select current_database();
> -- output:
> -- postgres
> ```

> [!TIP]
>
> ### To Check current DB User :
>
> ```sql
> select current_user;
> -- output:
> -- postgres
> ```

> [!IMPORTANT]
>
> ### To Check ServerIP and Port of current session:
>
> ```sql
> select inet_server_addr() ,inet_server_port();
> --output:
> --null [as it is connected through socket]
> ```
>
> ```sql
> postgres=# \conninfo
> You are connected to database "postgres" as user "postgres" via socket in "/tmp" at port "5432".
> ```

> [!TIP]
>
> ### To Check PostgreSQL version on Server:
>
> ```sql
> select version();
> --output:
> PostgreSQL 16.3 on x86_64-pc-linux-musl, compiled by gcc (Alpine 13.2.1_git20240309) 13.2.1 20240309, 64-bit
> ```
---
## Enabling Access for Network / Remote User

> [!IMPORTANT]
>
> There are 2 config files through which we can modify access for network and remote user:
>
> 1. **`postgresql.conf`**
> 1. **`pg_hba.conf`**
>
> For my postgres image those 2 files are present in below path :
>
> ```bash
> postgres@172.17.0.2:/postgres ]>ls
> Database  install
> postgres@172.17.0.2:/postgres ]>find . -iname "postgresql.conf"
> ./Database/postgresql.conf
> postgres@172.17.0.2:/postgres ]>find . -iname "pg_hba.conf"
> ./Database/pg_hba.conf
> ```

> [!IMPORTANT]
> in **postgresql.conf** add below line to configure postgres to listen from all hosts
> **`listen_addresses='*'`**
>
> Restart PostgreSQL server

> [!IMPORTANT]
> in **pg_hba.conf** add below line to allow access to **all database** to **all user**
>
> ```conf
> # TYPE  DATABASE        USER            ADDRESS                 METHOD
> host    all              all         0.0.0.0/0 md5
> ```
---
## Use the `psql` query and scripting

> [!IMPORTANT]
>
> To Create user and database through psql , first connect to db
>
> ```sql
> CREATE USER pritam WITH PASSWORD 'admin123';
> CREATE DATABASE mytestdb;
> GRANT ALL PRIVILEGES ON DATABASE mytestdb TO pritam;
> ```
>
> ![example_1.png Not Found](./img/example_1.png)

> [!NOTE]
> To Check Current date & time using psql:
>
> ```sql
> mytestdb2=# select current_time;
>    current_time
> --------------------
> 18:08:17.424516+00
> (1 row)
>
> mytestdb2=# select current_date;
> current_date
> --------------
> 2024-07-21
> (1 row)
> ```

> [!NOTE]
> To run sql from file using psql:
> first save sql to a file :
>
> ```bash
> d2b750b501aa:~$ ls
> d2b750b501aa:~$ echo "SELECT * FROM pg_stat_activity;" > test.sql
> d2b750b501aa:~$ cat test.sql
> SELECT * FROM pg_stat_activity;
> ```
>
> then psql -f <filename>
>
> Example(it's ran from client container so looks like this ):
>
> ```bash
> d2b750b501aa:~$ echo "SELECT current_time,current_date;" > test.sql
> d2b750b501aa:~$ psql postgres://postgres:postgres@pg_server:5432/postgres -f test.sql
>    current_time    | current_date
> --------------------+--------------
> 18:14:01.094701+00 | 2024-07-21
> (1 row)
> ```
>
> Another option is to connect psql first and then run like below to run sql from file
>
> ```sql
> postgres-# \i test.sql
>    current_time    | current_date
> --------------------+--------------
> 18:17:08.691451+00 | 2024-07-21
> (1 row)
> ```

> [!TIP]
> In psql we can enter 2 kind of commands :
>
> 1. psql meta commands
> 1. sql commands
>
> All Lines that starts with -> \ (backslash) are presumed to be the meta commands.

> [!NOTE]
>
> 1. `\?` : This provides help on psql meta commands
> 1. `\h` : This provides help on specific `SQL` commands

> [!NOTE]
>
> To Check if Postgres server is up or not:
>```sh
>pg_isready -h pg_server -U postgres -d postgres
>```
> output:
> > pg_server:5432 - accepting connections
>
>```sh
>d2b750b501aa:/$ pg_isready -h pg_server -U postgres -d postgres
>pg_server:5432 - accepting connections
>d2b750b501aa:/$ pg_isready -h pg_server -U postgres
>pg_server:5432 - accepting connections
>d2b750b501aa:/$ pg_isready -h pg_server
>pg_server:5432 - accepting connections
>```


> [!IMPORTANT]
>
> To check this below topics
>
> 1. **Information Function**
> 1. Output formatting
> 1. Execution timing using **`\timing`** commands
> 1. **Input/Output** and **Editing** commands
> 1. Automatic startup file : **`.psqlrc`**
> 1. Substitutable parameter (_variable_)
> 1. Access to the **OS command line**

---
## Changing Password Securely

>[!IMPORTANT]
> Once Connected to psql with user creds, we can change password for that user in interactive way using `\password` command.
> ![example_2.png not found](./img/example_2.png)