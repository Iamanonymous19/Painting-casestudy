

# Museum Art Database SQL Queries

This repository contains SQL queries designed to interact with a database containing information about museums, artworks, artists, and related data. The queries are aimed at retrieving various insights and analytics from the database.

## Introduction

The SQL queries provided in this repository are intended to be used with a relational database management system (RDBMS) containing tables representing different entities related to museums and art. The database schema typically includes tables such as `artist`, `work`, `museum`, `canvas_size`, `subject`, `product_size`, and `museum_hours`.

## Queries

The repository includes a set of SQL queries categorized based on the type of information they retrieve or the analysis they perform.

### 1. Data Retrieval Queries

#### 1.1. Paintings Not Displayed in Any Museum
```sql
SELECT * FROM work WHERE museum_id IS NULL;
```

#### 1.2. Museums Without Any Paintings
```sql
SELECT * FROM museum m
WHERE NOT EXISTS (
    SELECT 1 FROM work w
    WHERE w.museum_id = m.museum_id
);
```

#### 1.3. Paintings with Asking Price Greater Than Regular Price
```sql
SELECT * FROM product_size
WHERE sale_price > regular_price;
```

### 2. Data Analysis Queries

#### 2.1. Top 10 Most Famous Painting Subjects
```sql
SELECT s.subject, COUNT(*) AS no_of_paintings
FROM work w
JOIN subject s ON s.work_id = w.work_id
GROUP BY s.subject
ORDER BY no_of_paintings DESC
LIMIT 10;
```

#### 2.2. Top 5 Most Popular Museums
```sql
SELECT m.name AS museum, COUNT(*) AS no_of_paintings
FROM work w
JOIN museum m ON m.museum_id = w.museum_id
GROUP BY m.name
ORDER BY no_of_paintings DESC
LIMIT 5;
```

#### 2.3. Top 5 Most Popular Artists
```sql
SELECT a.full_name AS artist, COUNT(*) AS no_of_paintings
FROM work w
JOIN artist a ON a.artist_id = w.artist_id
GROUP BY a.full_name
ORDER BY no_of_paintings DESC
LIMIT 5;
```

### 3. Data Maintenance Queries

#### 3.1. Delete Duplicate Records
```sql
DELETE FROM work 
WHERE ctid NOT IN (
    SELECT MIN(ctid)
    FROM work
    GROUP BY work_id
);

-- Similar queries for product_size, subject, and image_link tables
```

#### 3.2. Remove Invalid Museum Hours Entry
```sql
DELETE FROM museum_hours 
WHERE ctid NOT IN (
    SELECT MIN(ctid)
    FROM museum_hours
    GROUP BY museum_id, day
);
```

---

## Usage

To use these SQL queries, follow these steps:

1. **Database Setup**: Ensure that you have a compatible RDBMS installed, such as MySQL, PostgreSQL, or SQLite. Create a database and import the provided dataset if available.

2. **Query Execution**: Copy the desired SQL query from the repository and execute it in your RDBMS environment against the database containing the relevant tables.

3. **Review Results**: Review the results returned by the query to verify that it provides the expected information or analysis.

4. **Customization**: Modify the queries as needed to suit your specific requirements or dataset structure.

## Contributing

Contributions to this repository are welcome. If you have additional SQL queries or improvements to existing queries, follow these steps to contribute:

1. Fork the repository.
2. Create a new branch for your feature or improvement (`git checkout -b feature/new-query`).
3. Make your changes and commit them (`git commit -am 'Add new query'`).
4. Push your changes to your fork (`git push origin feature/new-query`).
5. Open a pull request to the main repository.

## Conclusion

This repository provides a collection of SQL queries tailored for interacting with a museum art database. By leveraging these queries, users can gain valuable insights into various aspects of the art world, including popular painting subjects, renowned artists, and the distribution of artworks among museums.
