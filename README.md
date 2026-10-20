# pgstep

Step through PostgreSQL query execution clause by clause, on your own schema, entirely in the browser.

pgstep runs a real PostgreSQL 18 engine ([PGlite](https://github.com/electric-sql/pglite), compiled to WebAssembly) inside a single HTML page. Paste your tables and data, write a query, and watch the intermediate table at each stage of execution. No server, no install, no sign-up.

## Features

- **Execution order, step by step:** `WITH` → `FROM` → each `JOIN` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `DISTINCT` → `ORDER BY` → `LIMIT / OFFSET / FETCH` → result.
- **Visual cues:** filtered rows struck out, rows color-banded by group, outer-join rows padded with `NULL` marked, row counts per stage.
- **Your own schema:** any `CREATE TABLE` / `INSERT` script, real Postgres semantics, functions and error messages.
- **SQL editors:** syntax highlighting for keywords, functions, types, strings, numbers and comments, in light and dark mode.
- **Data changes:** `INSERT`, `UPDATE`, `DELETE`, `MERGE` show before/after with new and changed rows highlighted; `ALTER`, `CREATE` and `DROP` show the structure before and after.
- **Set operations:** each `UNION` / `INTERSECT` / `EXCEPT` branch, then the combined result.
- **Playback:** animated stages with Play, Previous / Next, keyboard arrows and adjustable speed.
- **Sandboxed runs:** each query runs in a transaction that is rolled back, unless *Keep changes* is on.

## Usage

Open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari).

1. Write your schema and data in the left editor, then click **Load schema**.
2. Write a query in the right editor and press **Ctrl + Enter**.
3. Step through the stages, or press **Play**.

Your schema, query and speed setting are saved in the browser's local storage.

## How it works

The page is self-contained: the PGlite WebAssembly binaries are embedded gzipped and base64-encoded (about 7 MB), and the PGlite JavaScript module (v0.5.8) is loaded from jsDelivr. Each stage is produced by running a rewritten version of your query against the database. For example, the `WHERE` stage evaluates the condition as an extra column, so removed rows can be shown instead of discarded.

## Limitations

- `GROUPING SETS`, `ROLLUP`, `CUBE` and `FETCH … WITH TIES` appear only in the final result, not as separate stages.
- With several statements in the editor, the earlier ones run first and only the last one is visualized.
- Display is capped at 1,000 rows per stage.

## License

MIT. PGlite is licensed under Apache 2.0 / the PostgreSQL License.
