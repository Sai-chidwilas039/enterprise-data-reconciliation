# Enterprise Data Reconciliation Pipeline

Python + SQL pipeline that merges three messy sources (a SQL user-history
database, 100K raw server logs, and daily FX rates) into a CFO revenue dashboard.

## Challenges solved
1. **Versioned data (SQL):** used ROW_NUMBER() OVER (PARTITION BY ...) to keep
   each user's latest status; filtered to Active users (25,000 → 14,183).
2. **Unstructured text:** extracted date, user, product and EUR value from raw
   logs with vectorized regex; removed ~5,000 ERROR rows.
3. **Time-series gaps:** forward-filled weekend and holiday FX rates so no
   transactions were lost in the join (104 missing rates handled).
4. **Scale:** 100,000 rows processed in ~1 second, no loops.

## Results
- 94,902 valid transactions → 53,899 from active users
- Monthly revenue stable at ~$12M; lowest in February (~$11.4M)
- Top 5 customers each spent ~$39K (low concentration risk)

## Tools
Python, pandas, SQLite, regex, matplotlib/seaborn
