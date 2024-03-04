# DOCKER SETUP

1. Create a `.env` file at the root directory and setup environment variables for [PostgresDB](https://hub.docker.com/_/postgres) and [Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/).
   
   E.g. 
   ```
    # POSTGRES
    POSTGRES_DB=GRAFANA_DB
    POSTGRES_USER=grafana
    POSTGRES_PWD=Q4sjxOWLaAIG
   ```

Run services ` docker compose up -d` 

# DOKKU DEPLOYMENT

## Postgres Setup

1. Install [Postgres plugin](https://github.com/dokku/dokku-postgres) `sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git postgres` on Dokku version **0.19.x+**

2. Create database `dokku postgres:create <my-grafana-db>`.
   
   You can override the default user password using the `-p` option

3. Link database to app `dokku postgres:link <my-grafana-db> <my-grafana-app>`

This will generate a **DATABASE_URL** environment variable attached to the app in the form `DATABASE_URL=postgres://user:password@host:port/db`. 

You can confirm this by running `dokku config:show <my-grafana-app>`.

## Grafana DB config
Configure the grafana app to connect to the service.
`dokku config:set <my-grafana-app> GF_DATABASE_TYPE=postgres GF_DATABASE_URL=<DATABASE_URL>`

This will restart the app with the new settings

***Security**: Always change the default Grafana **admin** password which is `admin`. You can achieve this by setting the environment variable **GF_SECURITY_ADMIN_PASSWORD**



