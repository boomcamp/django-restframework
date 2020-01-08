### Connecting to docker postgresql.

This is just the same on how you connect node to docker in your `frontend-course`.

### Steps

1. Configure `tutorial/api/settings.py` with these database settings.

```
DATABASES = {
     'default': {
        'ENGINE': 'django.db.backends.postgresql',  # The engine type
        'NAME': 'node3db',                          # Database name
        'USER': 'postgres',                         # Database username
        'PASSWORD' : 'node3db',                     # database password
        'HOST': 'localhost',                        # localhost
        'PORT': 5432                                # default postgres port
    }
}

```

2. We will need to install `psycopg2-binary` adapter to connect django into posgtres.

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ pipenv install psycopg2-binary
Installing psycopg2-binary…
Adding psycopg2-binary to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock (31f92c) out of date, updating to (622a82)…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (31f92c)!
Installing dependencies from Pipfile.lock (31f92c)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 22/22 — 00:00:05
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.

```

3. Now activate shell environment `pipenv shell`.


4. Create `docker-compose.yml` or copy it from https://github.com/boomcamp/node-3/blob/master/docker-compose.yml

```
version: '3'
services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: node3db
    ports:
      - 5432:5432
    volumes:
      - node3db:/var/lib/postgresql/data
volumes:
  node3db:
```

5. Run `docker-compose.yml`

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ docker-compose up
Creating network "tutorial_default" with the default driver
Creating volume "tutorial_node3db" with default driver
Pulling db (postgres:)...
latest: Pulling from library/postgres
b8f262c62ec6: Pull complete
fe6da876d968: Pull complete
46b9d53972f5: Pull complete
23a11bddcc75: Pull complete
d6744ba78bdc: Pull complete
8d95423a7aa9: Pull complete
8590ba4183e5: Pull complete
ed97b9b8e039: Pull complete
7b311f33f382: Pull complete
76802d77aa81: Pull complete
d84ab156d91d: Pull complete
3e7bd63c53e6: Pull complete
66992dbe6d8f: Pull complete
cb2fe0741e74: Pull complete
Digest: sha256:1c2e0ae6fa018d8547413423200f4e518ddcb1514e8ce67701993dc7f4dd98bf
Status: Downloaded newer image for postgres:latest
Creating tutorial_db_1 ... done

```

6. Check the container if its already created. 

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
2e20e3c5179e        postgres            "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        0.0.0.0:5432->5432/tcp   tutorial_db_1
```

6. Connect to container id `2e20e3c5179e`, create database and login to `node3db` database.

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ docker exec -it 2e20e3c5179e bash 
root@2e20e3c5179e:/# psql -U postgres
psql (12.0 (Debian 12.0-1.pgdg100+1))
Type "help" for help.

postgres=# CREATE DATABASE node3db;
CREATE DATABASE
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 node3db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(4 rows)

postgres=# \c node3db
You are now connected to database "node3db" as user "postgres".
node3db=# 
```

7. Run django migration.

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, languages, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying languages.0001_initial... OK
  Applying languages.0002_auto_20191008_0906... OK
  Applying sessions.0001_initial... OK

```

8. You should now successfully created these tables.

```
node3db=# \dt
                     List of relations
 Schema |              Name              | Type  |  Owner   
--------+--------------------------------+-------+----------
 public | auth_group                     | table | postgres
 public | auth_group_permissions         | table | postgres
 public | auth_permission                | table | postgres
 public | auth_user                      | table | postgres
 public | auth_user_groups               | table | postgres
 public | auth_user_user_permissions     | table | postgres
 public | django_admin_log               | table | postgres
 public | django_content_type            | table | postgres
 public | django_migrations              | table | postgres
 public | django_session                 | table | postgres
 public | languages_language             | table | postgres
 public | languages_paradigm             | table | postgres
 public | languages_programmer           | table | postgres
 public | languages_programmer_languages | table | postgres
(14 rows)

node3db=# 
```

9. Done.

### References

- [Connecting to a docker postgresql container](https://github.com/jinolacson/git-demonstration/blob/master/connect-docker-postgres/connect-docker-postgres.gif)

- [psql commands](http://postgresguide.com/utilities/psql.html)


