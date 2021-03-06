# Getting Started

## Initial setup for the database

- Obtain the Schema Server database initialization script from [here](sql/public.pgsql). This dump already contains the schema `public` which serves as a schema register for this database.

- Choose the name for your database, e.g., `dss`.

- Execute the following commands:

```
createdb dss
psql dss < public.pgsql
```

## Obtain the data to be imported

- obtain a JSON file containing the shape info extracted from the endpoint (see [OBIS-SchemaExtractor](https://github.com/LUMII-Syslab/OBIS-SchemaExtractor)).

## Import the first schema

- Obtain the Schema Server schema template from [here](sql/empty_template.pgsql).

- Choose the name for your schema, e.g., `myendpoint`.

- Execute the following commands:

```
psql dss < _template.pgsql
psql -c "alter schema empty rename to myendpoint" dss
```

You can repeat these commands if you need to import another schema,

- create an environment file (copy `sample.env` to `.env`) and configure the following variables inside the `.env` file:
  - `DB_URL` – connection string to the Data Shape Server PostgreSQL database,
  - `DB_SCHEMA` – name of the schema where the shapes should be imported into
  - `INPUT_FILE` – name of the JSON file with the extracted information (see previous section).
  - `SCHEMA_NAME` – db name for the DSS schema
  - `SCHEMA_DISPLAY_NAME` – schema name for the UI
  - `SPARQL_URL` – URL for the SPARQL requests to the endpoint
  - `NAMED_GRAPH` – named graph (optional)
  - `PUBLIC_URL` – public web site for the endpoint (optional)

- ensure that `node.js` is installed

- run `npm install` from the command line to install the prerequisites

- run `node work.js` from the command line to start the import

## Namespace table prepopulation

If desired, the namespaces table `ns` can also be prepopulated.

For any prefixes encountered in input JSON, an existing `ns` entry will be used, if possible.

For missing `ns` entries, the prefix.cc API will be queried to get the short form of the prefix.

Still unknown prefixes will get autogenerated short forms like `auto_42`.

