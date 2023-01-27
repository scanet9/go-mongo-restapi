# go-hexagonal-api
![CI](https://github.com/sergicanet9/go-hexagonal-api/actions/workflows/ci.yml/badge.svg)
![CD](https://github.com/sergicanet9/go-hexagonal-api/actions/workflows/cd.yml/badge.svg)
![Coverage](https://img.shields.io/badge/Coverage-60.9%25-yellow)
[![Go Reference](https://pkg.go.dev/badge/github.com/sergicanet9/go-hexagonal-api.svg)](https://pkg.go.dev/github.com/sergicanet9/go-hexagonal-api)

REST API implementing Hexagonal Architecture (Ports & Adapters) making use of own [scv-go-tools](https://github.com/sergicanet9/scv-go-tools) package.
<br />
It consists in a set of endpoints for user management that can indistinctly work with either a MongoDB or PostgreSQL instance, using the same endpoints and business logic.

Provides:
- MongoDB and PostgreSQL decoupled implementations of the repository adapter for persistent storage
- Database migrations with Goose for PostgreSQL implementation
- CRUD functionalities for user management
- JWT authentication and claim-based authorization
- Swagger UI documentation
- Unit tests with code coverage
- Integration tests for happy path
- Multi-environment JSON config files
- Dockerized app and Kubernetes Deployment
- CI/CD with Github Actions
- Async process for periodical health checking

## Run it with docker
```
make up
```
It starts 4 containers. Two of them are databases (MongoDB and PostgreSQL) and the other two are instances of the API, each of them already set up to work with one of the databases.
<br />
Both Swagger URLs are printed when running the command.

### Stop and remove the running containers
```
make down
```

## Debug it with VS Code
Debugging configurations provided in [launch.json](https://github.com/sergicanet9/go-hexagonal-api/blob/main/.vscode/launch.json) for hitting both MongoDB and PostgreSQL containers. Just select the desired one in the VS Code´s build-in debugger and run it.
<br />
Then open `http://localhost:{port}/swagger/index.html`, where `{port}` is the value specified in [launch.json](https://github.com/sergicanet9/go-hexagonal-api/blob/main/.vscode/launch.json) for the selected configuration.
<br />
<br />
NOTES:
- The target database container needs to be up and running.

## Run it with command line
```
    go run cmd/main.go --ver={version} --env={environment} --port={port} --db={database} --dsn={dsn}
```
or:
```
go build cmd/main.go
 ./main --ver={version} --env={environment} --port={port} --db={database} --dsn={dsn}
```
Provide the desired values to `{version}`, `{environment}`, `{port}`, `{database}`, `{dsn}`.
<br />
Then open `http://localhost:{port}/swagger/index.html`.
<br />
<br />

## Run unit tests with code coverage
```
make test-unit
```

### View coverage report
```
make cover
```

## Run integration tests
```
make test-integration
```
 NOTES:
- Docker is required for running integration tests.

## (Re)Generate Swagger documentation
```
make swagger
```
## (Re)Generate Mockery mocks
```
make mocks
```

## Database commands for Postgres
### Create new migration
```
make goose
```
Write the file name without ".sql" suffix and press enter.
Then edit the newly created file to define the behavior of the migration.

### Connect to database
```
psql "{dsn}"
```

### Create new database
```
CREATE DATABASE {db_name};
```

### Drop database (Azure Postgres Flexible Server)
```
az login
az postgres flexible-server db delete -g {resource_group} -s {resource_name} --database-name {db_name}
```

### Dump database schema
```
pg_dump -h {host} -U {username} {db_name} --schema-only > dump.sql
```

## Live AKS environments (turned off)
### Dev
http://go-hexagonal-api-mongo-dev.westeurope.cloudapp.azure.com/swagger/index.html
<br />
http://go-hexagonal-api-postgres-dev.westeurope.cloudapp.azure.com/swagger/index.html

## Author
Sergi Canet Vela

## License
This project is licensed under the terms of the MIT license.
