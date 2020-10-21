# ROW TYPE

> ROWTYPE atau sebaris kumpulan kolom sangat berbeda dengan kolom pada
> konteks membanding nilai `NULL`.

Query:

```sql
SELECT makanan_harga,
  makanan_harga IS NULL AS "is null",
  makanan_harga IS NOT NULL AS "is not null",
  makanan_harga IS DISTINCT FROM NULL AS "is distinct from null",
  makanan_harga IS NOT DISTINCT FROM NULL AS "is not distinct from null"
FROM (
  VALUES
    (ROW('Rujak Cingur', 9000::int, .10::float)),
    (ROW('Rujak Cingur', NULL::int, .10::float)),
    (ROW(NULL::text, NULL::int, NULL::float)),
    (NULL)
) AS ALIAS(makanan_harga)
;
```

Hasil query:

```sql
makanan_harga            |is null|is not null|is distinct from null|is not distinct from null|
-------------------------|-------|-----------|---------------------|-------------------------|
("Rujak Cingur",9000,0.1)|false  |true       |true                 |false                    |
("Rujak Cingur",,0.1)    |false  |false      |true                 |false                    |
(,,)                     |true   |false      |true                 |false                    |
                         |true   |false      |false                |true                     |
```

Rekomendasi:

> Sangat dianjurkan saat membandingkan baris data/rowtype, memilih kolomnya saja.
> contoh:

```sql
  if makanan_harga.nama is not null then ... end if;
```

Refs:  

[^1]: [functions-comparisons](https://www.postgresql.org/docs/9.6/functions-comparisons.html)
