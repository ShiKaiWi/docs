# ALTER TABLE

`ALTER TABLE` can change the schema or options of a table.

## ALTER TABLE SCHEMA

CeresDB current supports `ADD COLUMN` to alter table schema.

```sql
-- create a table and add a column to it
CREATE TABLE `t`(a int, t timestamp NOT NULL, TIMESTAMP KEY(t)) ENGINE = Analytic;
ALTER TABLE `t` ADD COLUMN (b string);
```

It now becomes:

```
-- DESCRIBE TABLE `t`;

name    type        is_primary  is_nullable is_tag

t       timestamp   true        false       false
tsid    uint64      true        false       false
a       int         false       true        false
b       string      false       true        false
```

## ALTER TABLE OPTIONS

CeresDB current supports `MODIFY SETTING` to alter table schema.

```sql
-- create a table and add a column to it
CREATE TABLE `t`(a int, t timestamp NOT NULL, TIMESTAMP KEY(t)) ENGINE = Analytic;
ALTER TABLE `t` MODIFY SETTING write_buffer_size='300M';
```

It now becomes:

```
CREATE TABLE `t` (`tsid` uint64 NOT NULL, `t` timestamp NOT NULL, `a` int, PRIMARY KEY(tsid,t), TIMESTAMP KEY(t)) ENGINE=Analytic WITH(arena_block_size='2097152', compaction_strategy='default', compression='ZSTD', enable_ttl='true', num_rows_per_row_group='8192', segment_duration='', storage_format='AUTO', ttl='7d', update_mode='OVERWRITE', write_buffer_size='314572800')
```
