# Introduction

This is a reproduction of https://github.com/prisma/prisma/issues/3715

## To Reproduce

1. Clone and fill your local postgres info in docker-compose file
2. Run these sql queries
```sql
CREATE TABLE table_a (
  id uuid NOT NULL PRIMARY KEY,
  name text
);

CREATE TABLE table_b (
  id uuid NOT NULL PRIMARY KEY,
  a_id uuid references table_a(id)
);

insert into table_a (id, name) values ('ab6981ee-02e8-4d3c-87a1-08d45ebd8b03', 'test');
insert into table_b (id, a_id) values ('5ac8367d-702e-4eff-b3fe-edfd9490c967', null);
```
3. Start the container and deploy the datamodel
4. Run the following query
```graphql
query {
  tableBs {
    id
    a {
      id
    }
  }
}
```
