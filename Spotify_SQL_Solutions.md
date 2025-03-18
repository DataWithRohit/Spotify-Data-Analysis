# Spotify SQL Solutions

## Easy Level

### 1. Retrieve the names of all tracks that have more than 1 billion streams.
**Solution:**
```sql
SELECT track 
FROM spotify 
WHERE stream > 1000000000;
```

### 2. List all albums along with their respective artists.
**Solution:**
```sql
SELECT DISTINCT album, artist 
FROM spotify;
```

### 3. Get the total number of comments for tracks where `licensed = TRUE`.
**Solution:**
```sql
SELECT SUM(comments) AS total_comments 
FROM spotify 
WHERE licensed = TRUE;
```

### 4. Find all tracks that belong to the album type `single`.
**Solution:**
```sql
SELECT track 
FROM spotify 
WHERE album_type = 'single';
```

### 5. Count the total number of tracks by each artist.
**Solution:**
```sql
SELECT artist, COUNT(track) AS total_tracks 
FROM spotify 
GROUP BY artist;
```

## Medium Level

### 1. Calculate the average danceability of tracks in each album.
**Solution:**
```sql
SELECT album, AVG(danceability) AS avg_danceability 
FROM spotify 
GROUP BY album;
```

### 2. Find the top 5 tracks with the highest energy values.
**Solution:**
```sql
SELECT track, energy 
FROM spotify 
ORDER BY energy DESC 
LIMIT 5;
```

### 3. List all tracks along with their views and likes where `official_video = TRUE`.
**Solution:**
```sql
SELECT track, views, likes 
FROM spotify 
WHERE official_video = TRUE;
```

### 4. For each album, calculate the total views of all associated tracks.
**Solution:**
```sql
SELECT album, SUM(views) AS total_views 
FROM spotify 
GROUP BY album;
```

### 5. Retrieve the track names that have been streamed on Spotify more than YouTube.
**Solution:**
```sql
SELECT track 
FROM (
    SELECT track, 
        SUM(CASE WHEN most_played_on ILIKE '%youtube%' THEN stream END) AS streamed_on_youtube,
        SUM(CASE WHEN most_played_on ILIKE '%spotify%' THEN stream END) AS streamed_on_spotify
    FROM spotify 
    GROUP BY track
) AS track_streams 
WHERE streamed_on_spotify > streamed_on_youtube AND streamed_on_youtube IS NOT NULL;
```

## Advanced Level

### 1. Find the top 3 most-viewed tracks for each artist using window functions.
**Solution:**
```sql
SELECT artist, track, views 
FROM (
    SELECT artist, track, views, 
           RANK() OVER (PARTITION BY artist ORDER BY views DESC) AS rank_no 
    FROM spotify
) AS ranked_tracks 
WHERE rank_no <= 3;
```

### 2. Find tracks where the liveness score is above the average.
**Solution:**
```sql
SELECT track 
FROM spotify 
WHERE liveness > (SELECT AVG(liveness) FROM spotify);
```

### 3. Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.
**Solution:**
```sql
WITH energy_stats AS (
    SELECT album, MAX(energy) AS highest_energy, MIN(energy) AS lowest_energy
    FROM spotify 
    GROUP BY album
)
SELECT album, highest_energy - lowest_energy AS energy_difference 
FROM energy_stats 
ORDER BY energy_difference DESC;
```

### 4. Find tracks where the energy-to-liveness ratio is greater than 1.2.
**Solution:**
```sql
SELECT track 
FROM spotify 
WHERE liveness > 0 AND (energy / liveness) > 1.2;
```

### 5. Calculate the cumulative sum of likes for tracks ordered by the number of views using window functions.
**Solution:**
```sql
SELECT trac
