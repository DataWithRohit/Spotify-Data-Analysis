# Spotify Data Analysis 📌

## Project Category: Advanced

[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)



## Overview

This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

## Table Schema

```sql
-- Create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```

## Project Steps

### 1. Data Exploration

Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:

- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

### 2. Querying the Data

SQL queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

#### Easy Queries

1. Retrieve the names of all tracks that have more than 1 billion streams.
2. List all albums along with their respective artists.
3. Get the total number of comments for tracks where `licensed = TRUE`.
4. Find all tracks that belong to the album type `single`.
5. Count the total number of tracks by each artist.

#### Medium Queries

1. Calculate the average danceability of tracks in each album.
2. Find the top 5 tracks with the highest energy values.
3. List all tracks along with their views and likes where `official_video = TRUE`.
4. For each album, calculate the total views of all associated tracks.
5. Retrieve the track names that have been streamed on Spotify more than YouTube.

#### Advanced Queries

1. Find the top 3 most-viewed tracks for each artist using window functions.
2. Write a query to find tracks where the liveness score is above the average.
3. Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.

```sql
WITH cte AS (
    SELECT album,
           MAX(energy) AS highest_energy,
           MIN(energy) AS lowest_energy
    FROM spotify
    GROUP BY 1
)
SELECT album,
       highest_energy - lowest_energy AS energy_diff
FROM cte
ORDER BY 2 DESC;
```

4. Find tracks where the energy-to-liveness ratio is greater than 1.2.

```sql
SELECT track
FROM spotify
WHERE liveness > 0 AND (energy / liveness) > 1.2;
```

5. Calculate the cumulative sum of likes for tracks ordered by the number of views using window functions.

```sql
SELECT track, views,
       SUM(likes) OVER (PARTITION BY track ORDER BY views DESC) AS Total_likes
FROM spotify;
```

## Query Optimization Technique

### 1. Initial Query Performance Analysis Using `EXPLAIN`

- We analyzed query performance using the `EXPLAIN` function.
- The initial execution time was **7 ms**, and planning time was **0.17 ms**.
- **Screenshot Before Indexing:**

### 2. Index Creation on the `artist` Column

```sql
CREATE INDEX idx_artist ON spotify(artist);
```

### 3. Performance Analysis After Index Creation

- Execution time improved to **0.153 ms**, and planning time was **0.152 ms**.
- **Screenshot After Indexing:**

### 4. Graphical Performance Comparison

- **Performance graphs showing query time before and after indexing:**



## Technology Stack

- **Database**: PostgreSQL
- **SQL Techniques**: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
- **Tools**: pgAdmin 4, PostgreSQL, Kaggle

## How to Run the Project

1. Install PostgreSQL and pgAdmin.
2. Set up the database schema and tables.
3. Insert the sample data.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques.

## Next Steps

- **Visualize the Data**: Use Tableau or Power BI to create dashboards based on query results.
- **Expand Dataset**: Add more rows for broader analysis.
- **Advanced Querying**: Optimize queries for large datasets.

## Contributing

If you would like to contribute, fork the repository, submit pull requests, or raise issues.

## License

This project is licensed under the MIT License.


