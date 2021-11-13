# README

- [README](#readme)
  - [TLDR](#tldr)
    - [Clone Project](#clone-project)
  - [Launch database and seed data](#launch-database-and-seed-data)
  - [Config Neo4j driver](#config-neo4j-driver)
  - [Install dependencies](#install-dependencies)
    - [Seed Database](#seed-database)
    - [Run Server](#run-server)
    - [launch some Operations](#launch-some-operations)
  - [Docker Compose](#docker-compose)
    - [Build Local Image](#build-local-image)
    - [Spin docker stack](#spin-docker-stack)

## TLDR

### Clone Project

```shell
# clone repo
$ git clone https://github.com/koakh/TypescriptNodeNeo4jGraphqlLibraryStarter.git
```

## Launch database and seed data

> bellow steps are only required first time, `MATCH (a) DETACH DELETE a` query can be used more than one time to tearDown/drop database nodes/relationship's

1. launch [neo4j desktop](https://neo4j.com/download-neo4j-now)
2. create clean database
3. install APOC plugin to created database
4. start database
5. open **neo4j browser**, open `init.cypher` and copy index's section in neo4j browser

`init.cypher` snippet, paset bellow in **neo4j browser**

```cypher
CALL db.index.fulltext.createNodeIndex("postIndex", ["Post"],["title", "content"]);
CREATE CONSTRAINT constraint_blog_id ON (blog:Blog) ASSERT blog.id IS UNIQUE;
CREATE CONSTRAINT constraint_blog_name ON (blog:Blog) ASSERT blog.name IS UNIQUE;
CREATE CONSTRAINT constraint_user_id ON (user:User) ASSERT user.id IS UNIQUE;
CREATE CONSTRAINT constraint_user_email ON (user:User) ASSERT user.email IS UNIQUE;
```

## Config Neo4j driver

```shell
$ cp packages/neo4j-gqllib-starter/server/.env.example packages/neo4j-gqllib-starter/server/.env
# edit `packages/neo4j-gqllib-starter/server/.env` and configure neo4j host and password
$ code packages/neo4j-gqllib-starter/server/.env
```


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

> tested with node v16.4.1, tip use nvh or nvm to opt for version ex `nvh i v16.4.1`

### Seed Database

```shell
$ yarn run neo4j-gqllib-starter:seed
```

check seed data in **neo4j browser**

```cypher
MATCH (n) RETURN n
```

### Run Server

```shell
$ yarn run neo4j-gqllib-starter:server
```

### launch some Operations

open file `packages/neo4j-gqllib-starter/server/client.http` or go to apollo studio at <http://localhost:5000/graphql>

> `client.http` needs [REST Client Extension](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

ex for `SignUpMutation` operation

```
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 218
ETag: W/"da-pQjLWL3sW9H1xlbCZMMY6qvTh+g"
Date: Tue, 31 Aug 2021 16:45:40 GMT
Connection: close

{
  "data": {
    "signUp": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI3ZmE4Y2ZmMC0xMjZmLTQ2MTktYjI4My01MTY4M2ZjY2JlYjciLCJyb2xlcyI6WyJST0xFX1VTRVIiXSwiaWF0IjoxNjMwNDI4MzQwfQ.DQFWwBONwTiNaA33sEonlDkwSUstBqsvEeHXwrH5DDs"
  }
}
```

## Docker Compose

### Build Local Image

```shell
# build image
$ npm run neo4j-gqllib-starter:docker:build
....
Successfully built fbeec194e574
Successfully tagged koakh/neo4j-gqllib-starter-server:latest
```

### Spin docker stack

first go to neo4 desktop and stop database, else port will fire a conflit `listen tcp4 0.0.0.0:7687: bind: address already in use`

```shell
$ docker-compose up
Creating network "typescriptnodexpshare_default" with the default driver
Pulling caddy (caddy:2.4.3)...

# run
neo4j-gqllib-starter:docker:run
```
