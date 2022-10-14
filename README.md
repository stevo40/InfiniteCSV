# InfiniteCSV
Store/Search Large 100k+ cols

Based on parquet columnar format.

* Spark has column limit, so subdivide cols into chunks.
*  Ingest csv and push column chunks to per chunk parquet based repositories.
  *  Could use spark for this. 
*  Use 2nd database to reference which columns in which repositories.
*  Store in 3rd database.

Indexing thoughts:
*  Index parquet column so all possible values have unique boolean index
*  Boolean index is 0 or 1 for all rows.
  *  Either rle or binary space encoding? (Probably sparse or sparse inverted)
*  SQL type logic plan
*  Return boolean indexes for column/value pairs
  *  Individual pair fetch can be parallelised
*  Todo: boolean merge of indexes.
  *  Can also be parallelised.
*  Return matching row positions.
*  3rd database returns specific rows.

Open Questions:
*  How to update all columns at once (transactional or acid required?) because will cause problems if slippage.
*  When to update index
*  What cluster/run strategy (maybe easier on spark)
*  What format for storing boolean indexes?
  *  bsp node sequential updates (node 120 overwritten with replacement)
  *  graphql for storing node tree?
*  How to handle new / dropped columns?
*  Chunked rows for splitting workload?
