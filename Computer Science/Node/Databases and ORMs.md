# Databases & ORMs

Node talks to databases through **drivers** (raw SQL over a connection) or **ORMs/query builders** (Prisma, Sequelize, Mongoose, Knex, Drizzle) that map tables to typed objects and generate SQL. The craft is in **connection pooling, migrations, transactions, and indexing** — the parts that make a database-backed API fast and safe.

**The Intuition:** A driver is a phone line to the database — you speak SQL over it. An ORM is a translator + secretary: you speak in objects, it writes the SQL, manages the connection pool, and keeps your schema versioned. The connection pool is a phone bank: instead of dialing a fresh number per request (slow), you keep a handful of lines open and hand each request an available one.

## Drivers vs ORMs

```js
// Raw driver (pg / mysql2) — full control, you write SQL:
import pg from 'pg';
const pool = new pg.Pool({ connectionString: process.env.DATABASE_URL });

const { rows } = await pool.query('SELECT * FROM users WHERE id = $1', [id]);

// ORM (Prisma) — typed objects, generated SQL:
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

const user = await prisma.user.findUnique({ where: { id } });
```

| | Driver | ORM |
|---|---|---|
| Control | full SQL control | abstraction (can fight it) |
| Speed | faster, less overhead | slower on complex queries |
| Type safety | manual mapping | generated types (Prisma) |
| Migrations | hand-written SQL | built-in (Prisma migrate) |
| Learning curve | need SQL | learn the ORM API |

## Connection pooling — never one connection per request

```js
// WRONG — a new connection for every request (slow, exhausts the DB):
app.get('/api/users', async (req, res) => {
  const conn = await pg.connect();          // handshake per request — expensive!
  try { /* query */ } finally { conn.end(); }
});

// RIGHT — a shared pool of reused connections:
const pool = new pg.Pool({ max: 10, idleTimeoutMillis: 30_000 });
// each query borrows a connection, uses it, returns it.

app.get('/api/users', async (req, res) => {
  const { rows } = await pool.query('SELECT * FROM users LIMIT 50');
  res.json(rows);
});
```

**Why pools matter:** establishing a DB connection is a multi-step handshake (TCP + auth + protocol). Reusing pooled connections turns that cost from per-request to once-per-pool. Set a sane `max` (connections are a server resource) — too many pools or too high `max` exhausts the database.

## Transactions — all-or-nothing

```js
// Transfer money: debit + credit MUST both succeed or both roll back.
import pg from 'pg';

async function transfer(fromId, toId, amount) {
  const client = await pool.connect();        // borrow a dedicated connection
  try {
    await client.query('BEGIN');
    await client.query('UPDATE accounts SET balance = balance - $1 WHERE id = $2', [amount, fromId]);
    await client.query('UPDATE accounts SET balance = balance + $1 WHERE id = $2', [amount, toId]);
    await client.query('COMMIT');             // both applied atomically
  } catch (err) {
    await client.query('ROLLBACK');           // neither applied
    throw err;
  } finally {
    client.release();                         // return the connection
  }
}
```

**The intuition:** a transaction is an *atomic unit* — either every statement commits or none do. Half a money transfer (debited, not credited) is a database corruption. `BEGIN` → work → `COMMIT`/`ROLLBACK` is the contract. With Prisma: `prisma.$transaction([...])`.

## Migrations — version-controlled schema

```bash
# Prisma:
npx prisma migrate dev --name add_users_table   # create + apply
npx prisma migrate deploy                        # apply in production
npx prisma db push                               # sync dev without history

# Knex:
npx knex migrate:make create_users               # write up/down
npx knex migrate:latest
```

```prisma
// prisma/schema.prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  posts     Post[]
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}
```

**Why migrations matter:** they're the *source of truth* for your schema, applied in order everywhere — dev, staging, prod. Never hand-edit production tables; change the schema file, generate a migration, deploy it.

## Indexing — make reads fast

```sql
-- Without an index: full table scan for every lookup.
-- With an index: logarithmic lookup, like the index of a book.

CREATE INDEX idx_users_email ON users (email);
-- Now: WHERE email = 'x@y.z' uses the index.

-- Compound index — order matters:
CREATE INDEX idx_posts_author_created ON posts (author_id, created_at);
-- Serves: WHERE author_id = 7 ORDER BY created_at DESC
```

**The intuition:** an index is a sorted copy of a column (or columns) that lets the DB jump straight to matching rows instead of reading every row. Cost: writes get slower (the index must be maintained) and the index uses disk. Index what you *filter/sort/join on*, not everything.

## N+1 queries — the ORM trap

```js
// BAD — one query per post + one per author (1 + N queries):
const posts = await prisma.post.findMany();
for (const post of posts) {
  const author = await prisma.user.findUnique({ where: { id: post.authorId } });
}

// GOOD — one query, join included:
const posts = await prisma.post.findMany({
  include: { author: true },     // single SQL query with a JOIN
});
```

**The intuition:** N+1 is the classic ORM performance killer — loading a list, then loading related data *per item* in a loop. Always eager-load (`include`/`JOIN`) when you know you'll need the related data.

## Choosing: SQL vs NoSQL

```text
PostgreSQL / MySQL — relational:
  ✅ structured data, relationships, ACID, complex queries
  ✅ the default choice for most apps

MongoDB — document store:
  ✅ flexible schemas, nested documents, horizontal scaling
  ⚠️ joins/transactions are weaker (or newer)

Redis — in-memory cache/queue:
  ✅ caching, sessions, rate limits, pub/sub
  ❌ durability is not the point

Postgres is the safe default; reach for others for specific needs.
```

---

**Setup:** A users table with a unique email + a posts table, using Prisma.

**Solution:**
```prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
  posts     Post[]
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
  createdAt DateTime @default(now())
}
```

```js
const user = await prisma.user.create({
  data: { email: 'ada@example.com', name: 'Ada' },
});

const posts = await prisma.post.findMany({
  where: { published: true, authorId: user.id },
  orderBy: { createdAt: 'desc' },
  include: { author: true },
});
```

**Key insight:** the schema is the contract — types, relations, uniqueness enforced at the database level (not just in app validation). Prisma generates TypeScript types from it, so a field rename is a compile error, not a runtime surprise.

---

**Setup:** Create an order with its items in one atomic step.

**Solution:**
```js
await prisma.$transaction(async (tx) => {
  const order = await tx.order.create({ data: { userId } });
  await tx.orderItem.createMany({
    data: cart.map(item => ({ orderId: order.id, productId: item.productId, qty: item.qty })),
  });
  await tx.inventory.decrement({ where: { id: productId }, by: qty });
});
```

**Key insight:** if any step fails, the whole transaction rolls back — no order without items, no items without inventory decrement. Consistency beats convenience; `$transaction` is the guardrail.

---

**Setup:** Why does the API crawl at 1,000 users but zoom at 10,000 after adding one index?

**Solution:** Without an index on the lookup column, every query is a full table scan — linear in table size (1,000 rows × scan cost). With a B-tree index, lookup is logarithmic — 10,000 rows cost only slightly more than 1,000. The index turned an O(n) query into an O(log n) query.

**Key insight:** index the columns you filter, join, and sort on. Then verify with `EXPLAIN ANALYZE` — it shows whether the planner is using your index or scanning the table anyway.

---

## Practice (try before peeking)

1. Why a connection pool instead of a new connection per request?
2. What is N+1 — and how do you fix it in an ORM?
3. When would you reach for MongoDB/Redis over Postgres?

<details><summary>Answers</summary>

1. Establishing a connection is a costly handshake; doing it per request wastes time and can exhaust the DB's connection limit under load. A pool reuses a bounded set of connections — fast and controlled.
2. Loading a list, then loading related rows *per item* in a loop — 1 + N queries. Fix by eager-loading the relation in one query (`include` in Prisma, `JOIN`/`IN` in SQL).
3. MongoDB for flexible/nested document shapes you don't want to normalize, or horizontal sharding needs. Redis for caching, sessions, rate limiting, queues — anything needing sub-millisecond reads of transient data. Postgres for everything where you value ACID and relational integrity.

</details>

---

**Common traps:**
- New DB connection per request — pool it
- Hand-editing production schema — migrate it
- N+1 queries in ORM loops — eager-load
- Indexing everything (write slowdown) or nothing (read slowdown) — target the hot queries
- Running long transactions (locks held too long) — keep them short
- Forgetting `ORDER BY` / pagination on list queries — nondeterministic results

---
