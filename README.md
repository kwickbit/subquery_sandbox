# Subquery Financial Transactions Indexer

This repository contains an indexer based on Subquery that indexes financial transactions for compliance purposes. The way data is indexed can be easily customized using the mapping functions. Users can map and index data based on blocks, transactions, operations, events, and effects.

## Running Locally

To run the project on your local machine, follow these steps:

### Development Environment Prerequisites

1. **Node Setup**: Ensure you have Node v18 installed. The recommended installation method is via `nvm`.
2. **Docker Setup**: Ensure you've created the `docker compliance_kwickbit_paltalabs_network` network.
3. **Dependencies**: Run `npm install`.
4. **Generate types**: `npx subql codegen`.
5. **Build the project**: `npx subql build`.
6. **Docker-Compose**: Launch the project using `docker-compose up --build`.

## Production Deployment

For deploying in a production environment:

1. **Hosting**: Choose your preferred hosting platform (AWS, Google Cloud, Azure, etc.).
2. **Database Setup**: Instantiate one PostgreSQL 12 instance. This can be AWS RDS or an equivalent service on another cloud provider.
3. **GraphQL Engine**: Instantiate one docker container from the `onfinality/subql-query:latest` image which represents the graphql-engine service.
4. **Subquery Node**: Instantiate another docker container built from the `deploy/ecr/subquery-node/Dockerfile` within the current context `.`. This represents the subquery-node service.

### Configuration

- Provide PostgreSQL database connection settings to the other two containers using the environment variables: `DB_USER`, `DB_PASS`, `DB_DATABASE`, `DB_HOST`, and `DB_PORT`.

- Ensure the subquery-node service is instantiated before the graphql-engine service.

- When instantiating the graphql-engine service, provide the `--indexer` input argument with the endpoint of the subquery-node service. In development mode with `docker-compose`, the endpoint is `http://subquery-node:3000`.

---

This README provides clear instructions for setting up and deploying the project. Additional sections like "Contributing," "License," or "Acknowledgments" can be added as needed.


## Editing for your project

You can edit the project by changing the following files:

- The project manifest in `project.yaml` defines the key project configuration and mapping handler filters
- The GraphQL Schema (`schema.graphql`) defines the shape of the resulting data that you are using SubQuery to index
- The Mapping functions in `src/mappings/` directory are typescript functions that handle transformation logic

SubQuery supports various layer-1 blockchain networks and provides [dedicated quick start guides](https://academy.subquery.network/quickstart/quickstart.html) as well as [detailed technical documentation](https://academy.subquery.network/build/introduction.html) for each of them.


## Query your project

For this project, you can try to query with the following GraphQL code to get a grasp of how it works.
In local mode, just head to `localhost:3000` in your browser and paste the following query:

```graphql
{
  query {
    transfers(first: 5, orderBy: VALUE_DESC) {
      totalCount
      nodes {
        id
        date
        ledger
        toId
        fromId
        value
      }
    }
    accounts(first: 5, orderBy: SENT_TRANSFERS_COUNT_DESC) {
      nodes {
        id
        sentTransfers(first: 5, orderBy: LEDGER_DESC) {
          totalCount
          nodes {
            id
            toId
            value
          }
        }
        firstSeenLedger
        lastSeenLedger
      }
    }
  }
}
```

You can explore the different possible queries and entities to help you with GraphQL using the documentation draw on the right.
