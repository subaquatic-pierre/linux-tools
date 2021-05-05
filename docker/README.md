# Postgres

### Docs

[Installation](https://www.postgresqltutorial.com/install-postgresql-linux/)

## CLI

##### Switch role to postgres

    sudo -i -u postgres

##### Start client

    psql

##### List all users

    \du

##### List all databases

    \l

##### List available tables in database

    \dt

##### Describe table

    \d table_name

##### List available schema

    \dn

##### Switch to new database

    \c dbname username

- A new username can be specified after database name, if omitted current user is used

#####

    \dt
