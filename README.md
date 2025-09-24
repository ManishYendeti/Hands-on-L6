# Hands‑On L6

**Name**: Manish Yendeti 
**Student ID**: 801429980

## Project Summary
This project explores user listening habits and broader music trends (in a synthetic music streaming environment) by leveraging Spark’s Structured APIs. The output includes:

- Users’ most listened genres  
- Average listening durations per song  
- Genre loyalty metrics per user  
- Identification of “night‑owl” users (users active between midnight and 5 AM)

## Datasets & Input Format
The data is generated via `datagen.py` and includes two CSVs:

1. **listening_logs.csv**
   - `user_id`: unique identifier for each user  
   - `song_id`: unique identifier for each song  
   - `timestamp`: when the song was played (e.g. “2025-03-23 14:05:00”)  
   - `duration_sec`: how many seconds the song was listened to  

2. **songs_metadata.csv**
   - `song_id`  
   - `title`  
   - `artist`  
   - `genre` (e.g. Pop, Rock, Jazz, etc.)  
   - `mood` (e.g. Happy, Chill, Energetic, etc.)

## Repository Layout
```
hands_on_6/
│  datagen.py                # Script to generate the input CSV files  
│  main.py                   # Spark Structured API processing  
│  inputs/                   # Directory for input CSVs  
│    ├ listening_logs.csv  
│    └ songs_metadata.csv  
│  outputs/                  # Output result directories  
│  summary.json              # Metadata about generation parameters  
│  README.md                 # Project documentation  
```

## Output Organization
The Spark application will write results into `outputs/`, structured into subfolders per task:

- `user_favorite_genres/`  
- `avg_listen_time_per_song/`  
- `genre_loyalty_scores/`  
- `night_owl_users/`

## Tasks & Expected Outputs

| Task | Output Folder | Columns / Description |
|---|---|---|
| 1. Determine each user’s favorite genre | `user_favorite_genres/` | `user_id, genre, plays` |
| 2. Compute average listen time per song | `avg_listen_time_per_song/` | `song_id, avg_duration_sec, title, artist, genre` |
| 3. Compute genre loyalty score per user | `genre_loyalty_scores/` | `user_id, total_plays, favorite_genre, fav_genre_plays, genre_loyalty_score` |
| 4. Identify night‑owl users | `night_owl_users/` | `user_id, night_plays, total_plays, night_proportion` |

### Example output snippets

- *user_favorite_genres*:
```
U0001  R&B  2
U0002  Electronic  1
...
```

- *avg_listen_time_per_song*:
```
S0001  125.167  Song 1  Artist 21  Rock  
S0002  147.75   Song 2  Artist 24  Electronic  
...
```

- *genre_loyalty_scores*:
```
U0001  4  R&B  2  0.5  
U0002  4  Electronic  1  0.25  
...
```

- *night_owl_users*:
```
U0005  4  9  0.444444  
U0058  4  9  0.444444  
...
```

## How to Run

### Prerequisites
- Python 3.x (`python3 --version`)  
- PySpark (`pip install pyspark`)  
- Apache Spark installed and on your PATH (`spark-submit --version`)  
- Java 8+ (required by Spark)

### Steps

1. **Generate input data**
   ```bash
   python3 datagen.py --out inputs/
   ```

2. **Run the Spark application**
   ```bash
   spark-submit main.py --listening inputs/listening_logs.csv --songs inputs/songs_metadata.csv --out outputs/
   ```

3. **Inspect output directories**
   ```bash
   ls outputs/
   ```

## Common Issues & Fixes

| Problem | Solution |
|---|---|
| PySpark not installed | `pip install pyspark` |
| Write permission denied | Check and fix permissions on `outputs/` directory |
| Java / JVM errors | Ensure Java (version 8 or above) is installed and set up |
