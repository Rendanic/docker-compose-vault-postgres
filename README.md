# Vault

## Installation

### Database initialization

    docker-compose -f compose-vault-postgres.yml up -d postgres
    docker-compose -f compose-vault-postgres.yml logs postgres

### Vault startup

    docker-compose -f compose-vault-postgres.yml up -d
    docker-compose -f compose-vault-postgres.yml logs -f vault

## Configuration

    docker-compose -f compose-vault-postgres.yml exec vault vault operator init

## Backup Database

    docker-compose -f compose-vault-postgres.yml exec -u postgres postgres pg_dumpall -U vault --if-exists --clean > pg_vault.dmp

## Restore Database

    docker-compose -f compose-vault-postgres.yml stop
    docker-compose -f compose-vault-postgres.yml up -d postgres
    docker-compose -f compose-vault-postgres.yml logs postgres

    docker cp pg_vault.dmp vault_postgres:/tmp
    docker-compose -f compose-vault-postgres.yml exec -T -u postgres postgres psql -U vault -d postgres -f /tmp/pg_vault.dmp

    docker-compose -f compose-vault-postgres.yml up -d
    docker-compose -f compose-vault-postgres.yml logs -f vault

    docker-compose -f compose-vault-postgres.yml exec vault vault status

## Postgres Login

    docker-compose -f compose-vault-postgres.yml exec -u postgres postgres psql -U vault
    docker-compose -f compose-vault-postgres.yml exec -u postgres postgres bash