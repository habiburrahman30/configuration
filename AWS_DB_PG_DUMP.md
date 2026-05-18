
---

## The Plan

The error (`type "Roles" already exists`) means your DB has partial schema state from a previous failed/manual migration. The `_prisma_migrations` table is out of sync.

---


## Step 1 — Backup the database

```bash
PGPASSWORD="your_password" pg_dump \
  -h database-1.c3c2quwug9uv.ap-northeast-3.rds.amazonaws.com \
  -U postgres \
  -d dtc \
  -Fc \
  -f "backup_$(date +%Y%m%d_%H%M%S).dump"
```


Great! Backup is done. Now **Step 2** — drop and recreate the schema:

```bash
PGPASSWORD="your_password" psql \
  -h database-1.c3c2quwug9uv.ap-northeast-3.rds.amazonaws.com \
  -U postgres \
  -d dtc \
  -c "DROP SCHEMA public CASCADE; CREATE SCHEMA public; GRANT ALL ON SCHEMA public TO public;"
```

You should see output like:
```
DROP SCHEMA
CREATE SCHEMA
GRANT
```

Then run **Step 3** — migrate:

```bash
npx prisma migrate deploy
```


Now **Step 4** — restore your data:

```bash
pg_restore \
  -h database-1.c3c2quwug9uv.ap-northeast-3.rds.amazonaws.com \
  -U postgres \
  -d dtc \
  --data-only \
  --disable-triggers \
  -W \
  backup_*.dump
```

It will prompt for your password. The `backup_*.dump` wildcard will pick up the file automatically — or replace it with the exact filename if you have multiple dumps.

---

Then **build and start**:

```bash
# Install dependencies (if needed)
npm install

# Build
npm run build

# Start
npm start
```

Or if you're using PM2:

```bash
npm run build
pm2 restart all
```

Let me know your output and what errors (if any) come up!
