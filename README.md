# 🎬 Netflix Movies & TV Shows Data Analysis using SQL
# 🚀 Unveiling trends in Netflix content to drive data-backed decision-making!

![](https://github.com/Palak019/netflix_sql_project/blob/main/netflix%20logo.jpg)

## 📌 Overview
Netflix dominates the global streaming industry, but what drives its content strategy? This project uncovers:

✔️ How Netflix’s content library evolved over time

✔️ Which regions dominate content production

✔️ The most popular genres & ratings

✔️ Trends in Movies vs. TV Shows

✔️ Business insights to optimize content strategies

## Objectives
✅ Unveil Content Evolution – Analyze Netflix’s content expansion and production trends over time.

✅ Decode Regional Dominance – Identify top content-producing countries shaping Netflix’s global library.

✅ Explore Genre & Rating Patterns – Discover audience preferences through genre popularity and rating trends.

✅ Contrast Content Formats – Compare the dominance of Movies vs. TV Shows across different years.

✅ Spotlight Creative Minds – Highlight top directors and analyze content duration trends.

✅ Empower Strategic Decisions – Deliver data-driven insights to optimize content curation and audience engagement. 🚀

## 📊 Dataset

 📥 Source: **Dataset Link:** [Netflix Kaggle Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)




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
## Business Problems and Solutions

## 1. Finding Duplicate Titles
```sql
SELECT title, COUNT(*) AS count
FROM netflix
GROUP BY title
HAVING count(*) > 1;
```
## 2. Total Number of Movies and TV Shows
```sql
select type, 
count(*) as total_count
from netflix
group by type;
```

## 3. Top 5 Countries with Most Content
```sql
select country,
count(*) as content_count
from netflix
where country is not null
group by country
order by content_count desc
limit 5;
```

## 4. Most Common Ratings
```sql
select rating,
count(*) as rating_count
from netflix
where rating is not null
group by rating 
order by rating_count desc
limit 5;
```

## 5. Year with the Most Releases
```sql
select release_year,
count(*) as release_count
from netflix
where release_year is not null
group by release_year
order by release_count  desc
limit 1;
```

## 6. Content Trend Over the Years
```sql
select release_year,
count(*) as content_count
from netflix
group by release_year
order by release_year asc;
```

## 7. Most Frequent Directors
```sql
select director,
count(*) as movie_count
from netflix
where director is not null
group by director
order by movie_count desc
limit 10;
```

## 8. Most Popular Genres
```sql
select listed_in as Genres,
count(*) as genre_count
from netflix
where listed_in is not null
group by listed_in
order by genre_count desc
limit 5;
```

## 9. Number of TV Shows vs Movies Per Year
```sql
select release_year,
		sum(case when type = 'Movie' then 1 else 0 end) as movie_count,
		sum(case when type = 'TV Show' then 1 else 0 end) as tv_show_count
from netflix
group by release_year
order by release_year;
```
			 
## 10. Most Common Movie Durations
```sql
select duration,
count(*) as DURATION_count
from netflix 
where type = 'Movie'
group by duration
order by DURATION_count desc
limit 5;
```

## 11. Countries with the Most TV Shows
```sql
select country,
count(type) as tv_show_count
from netflix
where type = 'TV Show' 
and country is not null
group by country 
order by tv_show_count desc;
```

## 12. Longest Running TV Shows
```sql
select title,
duration 
from netflix 
where type = 'TV Show'
order by duration desc
limit 5;
```

## 13. Countries Producing the Most Movies
```sql
select country,
count(type) as Movie_count
from netflix
where type = 'Movie'
and country is not null
group by country
order by Movie_count desc;
```

## 14. Percentage of Content by Type
```sql
select type,
count(*) as content_count,
round(count(*)*100.0/(select count(*) from netflix),2) as percentage
from netflix
group by type
order by content_count desc;
```

## 15. Directors with the Most TV Shows
```sql
select director,
count(type) as director_count
from netflix
where type = 'TV Show'
and director is not null
group by director
order by director_count desc
limit 5;
```

## 16. Most Recent Movie Releases
```sql
select title,
release_year
from netflix
where type = 'Movie'
order by release_year desc
limit 5;
```

## 17. Most Recent TV Shows
```sql
select title,
release_year
from netflix
where type = 'TV Show'
order by release_year desc
limit 5;
```

## 18. Directors Who Directed Multiple Genres
```sql
select director,
count(distinct listed_in) as total_genres
from netflix
where director is not null
group by director 
having count(distinct listed_in) > 1
order by  total_genres desc;
```

## 19. Content Available by Region
```sql
select country as Region,
count(*) as total_content
from netflix
where country is not null
group by country
order by total_content desc;
```

## 20. Number of Movies/TV Shows Added Per Month
```sql
SELECT SUBSTR(date_added, 1, POSITION(' ' IN date_added) - 1) AS month , count(*) as content_count
FROM netflix
WHERE date_added IS NOT NULL
GROUP BY month
ORDER BY content_count DESC;
```

## 21.Rank Countries by Content Production Each Year
```sql
select country,
release_year,
count(*) as content_count,
rank() over (partition by release_year ORDER BY count(*)  DESC) AS rank
from netflix
where country is not null
group by release_year , country
order by release_year desc , rank;
 ```

## 22.Find the Most Popular Rating Per Year
```sql
with rating_per_year as (
	select rating,
	release_year,
	count(*) as rating_count,
	rank() over(partition by release_year order by count(*) desc) as rank
	from netflix
	where rating is not null
	group by rating,release_year
)
select rating,
release_year,
rating_count
from rating_per_year
where rank = 1
order by release_year;
```
	
## 23.Find the Top 3 Most Common Genres for Each Year
```sql
with genre_rank as(
		select release_year,
		listed_in as genre,
		count(*) as genre_count,
		rank() over(partition by release_year order by count(*) desc) as rank
		from netflix
		group by release_year , genre
) 
select release_year,
genre,
genre_count
from genre_rank
where rank <= 3
order by release_year desc , rank;
```

## 24.Identify Countries with Consistent Content Growth
```sql
WITH content_growth AS (
    SELECT 
        country, 
        release_year, 
        COUNT(*) AS content_count,
        LAG(COUNT(*)) OVER (PARTITION BY country ORDER BY release_year) AS prev_year_count
    FROM netflix
    WHERE country IS NOT NULL
    GROUP BY country, release_year
)
SELECT 
    country, 
    release_year, 
    content_count, 
    prev_year_count,
    content_count - prev_year_count AS growth
FROM content_growth
WHERE prev_year_count IS NOT NULL
ORDER BY country, release_year DESC;
```



















