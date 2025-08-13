[![Quarto Publish](https://github.com/vanHeemstraSystems/task-management/actions/workflows/publish.yml/badge.svg)](https://github.com/vanHeemstraSystems/task-management/actions/workflows/publish.yml)

task-management
# Task Management

<img width="648" height="332" alt="Image" src="https://github.com/user-attachments/assets/0e221315-b143-4648-b3c2-93545cca233a" />

Can be read as "Task Management" at https://app.gitbook.com/s/Rs3XPuVclvoj92Exb9AA/

Can be browsed as "Task Management" at https://vanheemstrasystems.github.io/task-management/

Documentation of this repository is automatically done with Quarto using GitHub Actions as described at https://github.com/vanHeemstraSystems/quarto-to-github-pages/blob/main/300/300/README.md

Based on "How to Run PostgreSQL and pgAdmin Using Docker" at https://towardsdatascience.com/how-to-run-postgresql-and-pgadmin-using-docker-3a6a8ae918b5

Based on "Super Productivity" at https://github.com/johannesjo/super-productivity

![Image](https://github.com/user-attachments/assets/53516978-f490-44b9-ae86-15c2aea0de06)

Create yarn.lock file as follows:

```
$ cd containers/app/main
$ yarn install
```

Run as follows:

```
$ cd containers/app
$ docker-compose --file docker-compose.dev.yml --project-name task-management-dev up --build -d
```

**NOTE**: When you try to login to PostgreSQL database via **pgAdmin** (Recommended), use the following credentials:

- Email Address / Username: = Use the value of PGADMIN_DEFAULT_EMAIL_DEV/PROD as specified in the .env file
- Password: Use the value of PGADMIN_DEFAULT_PASSWORD_DEV/PROD as specified in the .env file

Inside pgAdmin register a new Server using the following credentials:

From the menu choose File > Register ... Server:

In tab **General**:

- Name: Use a name like "task-management-dev" (if for development) - REQUIRED

In tab **Connection**:

- Host name/address: localhost
- Port: Use the value of port as specified in the docker-compose file
- Maintenance database: postgres
- Username: Use the value of POSTGRES_USERNAME_DEV/PROD as specified in .env file
- Password: Use the value of POSTGRES_PASSWORD_DEV/PROD as specified in .env file

**NOTE**: When you try to login to PostgreSQL database via **Adminer**, use the following credentials:

- System: PostgreSQL
- Server: = IP address and exposed postgresql port (e.g. :5432) of the Docker host =
- Username: Postgres
- Password: = can be set or found in your environment variables file .env =
- Database: = leave this blank =

If you tick the "Permanent login" box, next time you visit the login page, your previous shortcut will be shown.
