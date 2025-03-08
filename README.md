# Netflix Movies and TV Shows Data Analysis using SQL

![](   https://github.com/Palak019/netflix_sql_project/blob/main/logo.jpg)

## Overview
This project unveils powerful insights into Netflix’s content evolution, highlighting production trends, top genres, ratings, and regional dominance. It delivers data-driven intelligence for strategic decision-making, enhancing content curation and audience engagement. 🚀

## Objectives
✅ Unveil Content Evolution – Analyze Netflix’s content expansion and production trends over time.

✅ Decode Regional Dominance – Identify top content-producing countries shaping Netflix’s global library.

✅ Explore Genre & Rating Patterns – Discover audience preferences through genre popularity and rating trends.

✅ Contrast Content Formats – Compare the dominance of Movies vs. TV Shows across different years.

✅ Spotlight Creative Minds – Highlight top directors and analyze content duration trends.

✅ Empower Strategic Decisions – Deliver data-driven insights to optimize content curation and audience engagement. 🚀

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## Schema

```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```
