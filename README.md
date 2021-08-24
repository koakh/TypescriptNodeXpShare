# README

## TLDR

### Clone Project

```shell
$ git clone https://github.com/koakh/TypescriptNodeNeo4jGraphqlLibraryStarter.git
```

## Launch database and seed data

> bellow steps are only required first time, `MATCH (a) DETACH DELETE a` query can be used more than one time to tearDown/drop database nodes/relationship's

1. launch [neo4j desktop](https://neo4j.com/download-neo4j-now)
2. create clean database
3. install APOC plugin to created database
4. open **neo4j browser**, open `init.cypher` and copy index's section in neo4j browser, for ex
5. start database

`init.cypher`

```cypher
CALL db.index.fulltext.createNodeIndex("postIndex", ["Post"],["title", "content"])
CREATE CONSTRAINT constraint_blog_id ON (blog:Blog) ASSERT blog.id IS UNIQUE;
CREATE CONSTRAINT constraint_blog_name ON (blog:Blog) ASSERT blog.name IS UNIQUE;
CREATE CONSTRAINT constraint_user_id ON (user:User) ASSERT user.id IS UNIQUE;
CREATE CONSTRAINT constraint_user_email ON (user:User) ASSERT user.email IS UNIQUE;
```

## Config Neo4j driver

```shell
$ cp packages/neo4j-gqllib-starter/server/.env.example packages/neo4j-gqllib-starter/server/.env
```

edit `packages/neo4j-gqllib-starter/server/.env` and configure neo4j host and password

```shell
NEO_USER=neo4j
NEO_PASSWORD=password
NEO_URL=neo4j://localhost:7687/neo4j
```

> confirm that have a valid running server

## Install dependencies

```shell
$ node -v
v16.4.1
$ yarn install
```

### Seed Database

```shell
$ yarn run neo4j-gqllib-starter:seed
```

### Run Server

```shell
$ yarn run neo4j-gqllib-starter:server
```

### launch some Operations

open file `packages/neo4j-gqllib-starter/server/client.http` or go to apollo studio at <http://localhost:5000/graphql>

> `client.http` needs [REST Client Extension](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
