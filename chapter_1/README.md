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
>
> [!DANGER]
> > Not Recommended
> > `PGPASSWORD=MyPassWord@123`

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
