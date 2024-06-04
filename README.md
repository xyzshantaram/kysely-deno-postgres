# kysely-deno-postgres

Connector for [Kysely](https://koskimas.github.io/kysely/index.html) with
[deno-postgres](https://deno-postgres.com). Fork of <https://github.com/Industrial/kysely-deno-postgres>.

## Usage

```ts
import {
  Kysely,
  PostgresAdapter,
  PostgresIntrospector,
  PostgresQueryCompiler,
} from "npm:kysely:0.27.3";
import { PostgreSQLDriver } from "https://gitlab.com/soapbox-pub/kysely-deno-postgres/-/raw/main/mod.ts";

const kysely = new Kysely({
  dialect: {
    createAdapter() {
      return new PostgresAdapter();
    },
    createDriver() {
      return new PostgreSQLDriver({
        // If connectionString is passed, other options are ignored.
        // connectionString: Deno.env.get('DATABASE_URL'),
        applicationName: "MyApp",
        connection: {
          attempts: 1,
        },
        database: "postgres",
        hostname: "localhost",
        host_type: "tcp", // "tcp" | "socket"
        password: "postgres",
        port: 5432,
        tls: {
          enabled: true,
          enforce: false,
          caCertificates: [],
        },
        user: "postgres",
      });
    },
    createIntrospector(db: Kysely<unknown>) {
      return new PostgresIntrospector(db);
    },
    createQueryCompiler() {
      return new PostgresQueryCompiler();
    },
  },
});
```
