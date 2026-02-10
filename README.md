The following other gotchas will be explained and code provided.

 
1. Eager v Lazy Evaluation
df.lazy().filter(...)  # Returns LazyFrame, doesn't execute
df.lazy().filter(...).collect()  # Actually executes

2. select() vs with_columns()
df.select(pl.col("a"))  # Returns ONLY column "a"
df.with_columns(pl.col("a"))  # Returns ALL columns plus modified "a"

3. Expression Context Matters
pl.col("a").sum()  # Works in select/group_by
pl.sum("a")  # Different! Use in different contexts

4. Row Indexing Different from Pandas
df[0]  # Returns first ROW as DataFrame (not Series!)
df[0, 0]  # Returns scalar value
df[:, 0]  # Returns first column

5. No Inplace Operations
df.sort("a")  # Doesn't modify df!
df = df.sort("a")  # Need to reassign

6. Column Names Must Be Strings
df.select(["a", "b"])  # ✅ List of strings
df.select("a", "b")  # ❌ Doesn't work like pandas

7. null vs NaN
pl.col("a").is_null()  # For None/NULL values
pl.col("a").is_nan()  # For NaN (only floats)
 

8. Casting Syntax
pl.col("a").cast(pl.Int64)  # ✅ Polars dtype
pl.col("a").cast(int)  # ❌ Won't work

9. String Operations Require .str
pl.col("name").str.to_uppercase()  # ✅
pl.col("name").to_uppercase()  # ❌

10. Join Behavior Defaults
df1.join(df2, on="id")  # Default is INNER join (like SQL)
 

11. Filter Requires Boolean Expression
df.filter(pl.col("age") > 30)  # ✅
df.filter("age > 30")  # ❌ String doesn't work

12. GroupBy Aggregation Syntax
df.group_by("city").agg(pl.col("salary").mean())  # ✅
df.group_by("city").mean()  # ❌ Need .agg()

13. Column Assignment in Expressions
.with_columns(new_col=pl.col("a") * 2)  # ✅ Keyword
.with_columns(pl.col("a") * 2)  # ❌ Needs alias()

14. Reading CSVs – Schema Inference
pl.read_csv("file.csv")  # May infer wrong types
pl.read_csv("file.csv", dtypes={"col": pl.Int64})  # Safer

15. Date/Time Parsing
pl.col(“date”).str.strptime(pl.Date, “%Y-%m-%d”) # ✅
pl.col(“date”).to_datetime() # ❌ Doesn’t exist
