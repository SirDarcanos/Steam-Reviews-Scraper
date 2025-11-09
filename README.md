# Steam Reviews Scraper

A lightweight Python utility to collect and export **Steam product reviews** (via the official appreviews API) into a clean CSV file — ideal for **NLP, sentiment analysis**, or data exploration.

Developed by [Nicola Mustone](https://nicolamustone.blog)  
Licensed under **GPLv2**.

---

## Features

- Fetches all accessible reviews for a given Steam app ID  
- Supports pagination via Steam’s cursor system  
- Exports structured CSV data for easy processing  
- Includes CLI options for filtering by language, review type, purchase type, and cursor continuation  
- Handles polite delays and avoids duplicates automatically  

## Requirements

- Python **3.9+**

## Usage

### Basic
```bash
python steam_reviews.py --app-id 381210
```

### Command options
| Flag                         | Description                                               | Default             |
| ---------------------------- | --------------------------------------------------------- | ------------------- |
| `--app-id`                   | **(Required)** Steam app ID to fetch reviews for          | —                   |
| `--out`                      | Output CSV file name                                      | `steam_reviews.csv` |
| `--delay`                    | Delay between requests (seconds)                          | `0.25`              |
| `--language`                 | Review language (`english`, `all`, etc.)                  | `all`               |
| `--purchase-type`            | Filter by purchase type (`all`, `steam`, `non_steam`)     | `all`               |
| `--review-type`              | Filter by review type (`all`, `positive`, `negative`)     | `all`               |
| `--filter-offtopic-activity` | Filter out off-topic activity (1 = yes, 0 = no)           | `0`                 |
| `--cursor`                   | Continue from a previous cursor token (`*` = start fresh) | `*`                 |

### Example
```bash
python steam_reviews.py \
  --app-id 500810 \
  --language english \
  --review-type all \
  --purchase-type all \
  --out arcanum_reviews.csv
```
## CSV Output

The script writes one row per review, making it easy to load into Pandas, Excel, or any NLP pipeline.

| Column                          | Description                                      |
| ------------------------------- | ------------------------------------------------ |
| `app_id`                        | Steam App ID                                     |
| `review`                        | The full review text                             |
| `sentiment`                     | `1` = positive review, `0` = negative            |
| `purchased`                     | `1` if purchased through Steam                   |
| `received_for_free`             | `1` if the reviewer got the game for free        |
| `votes_up`                      | Number of “helpful” votes                        |
| `votes_funny`                   | Number of “funny” votes                          |
| `date_created`                  | Review creation date (UTC, format: `YYYY-MM-DD`) |
| `date_updated`                  | Last update date (UTC, format: `YYYY-MM-DD`)     |
| `author_num_games_owned`        | Total number of games owned by the reviewer      |
| `author_num_reviews`            | Total number of reviews written by the reviewer  |
| `author_playtime_forever_min`   | Total playtime for the game (minutes)            |
| `author_playtime_at_review_min` | Playtime at the moment of review (minutes)       |

### Example Output (truncated)
```csv
app_id,review,sentiment,purchased,received_for_free,votes_up,votes_funny,date_created,date_updated,author_num_games_owned,author_num_reviews,author_playtime_forever_min,author_playtime_at_review_min
500810,"Amazing classic RPG. Deep mechanics, lots of bugs, but worth it.",1,1,0,42,3,2023-11-05,2023-11-06,214,15,1220,950
500810,"Controls are painful, but worldbuilding is fantastic.",1,1,0,18,2,2023-11-07,2023-11-07,96,4,300,220
500810,"Crashes constantly on modern PCs.",0,1,0,12,0,2023-11-10,2023-11-10,54,2,60,50
```

## License
This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License version 2.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE

Full text in LICENSE file.

## Notes
* Respect Steam’s API and Terms of Service: store.steampowered.com/subscriber_agreement
* Use reasonable delays (--delay) when fetching large datasets.
* Data represents user-generated content from Steam; handle responsibly.

