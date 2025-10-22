#systemDesign #db

- Database indexing is like putting an index at the back of a book — it creates a shortcut so the database doesn’t have to scan every single row to find what you’re asking for.
- the unique constraint automatically creates indexing for that column
- Every index helps reads, but hurts writes
