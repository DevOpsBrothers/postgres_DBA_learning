# How to Use this image for Practice PGSQL

> Author : Pritam 

Here postgres running as server , and we are connecting database from other container as client.

>[!NOTE]
>
> Step 1: Build Image
>```bash
>docker-compose build
>```
>
> Step 2: Run Containers
>```bash
>docker-compose up -d
>```
>
> Step 3: Log in to client container
>```bash
>docker-compose exec -it pg_client bash
>```
>
> Step 4: Connect to postgres server container from client using below command
>```bash
>docker compose exec -it pg_client bash
>```
>

>{!CAUTION}
>
> No Volume is attached with any of the container as it is only for learning purpose , so it won't retain any data once container are destroyed.

