### Table

Table: books

| Column Name |  Type   |
| :---------- | :-----: |
| book_id     |   int   |
| title       | varchar |
| author      | varchar |
| genre       | varchar |
| pages       |   int   |

book_id is the unique ID for this table.
Each row contains information about a book including its genre and page count.
Table: reading_sessions

| Column Name    |  Type   |
| :------------- | :-----: |
| session_id     |   int   |
| book_id        |   int   |
| reader_name    | varchar |
| pages_read     |   int   |
| session_rating |   int   |

session_id is the unique ID for this table.
Each row represents a reading session where someone read a portion of a book. session_rating is on a scale of 1-5.
Write a solution to find books that have polarized opinions - books that receive both very high ratings and very low ratings from different readers.

A book has polarized opinions if it has at least one rating ≥ 4 and at least one rating ≤ 2
Only consider books that have at least 5 reading sessions
Calculate the rating spread as (highest_rating - lowest_rating)
Calculate the polarization score as the number of extreme ratings (ratings ≤ 2 or ≥ 4) divided by total sessions
Only include books where polarization score ≥ 0.6 (at least 60% extreme ratings)
Return the result table ordered by polarization score in descending order, then by title in descending order.

<hr>

### Problem: 3642. Find Books with Polarized Opinions

<hr>

### SQL Solution

1. First solution

```sql
--RunTime 507ms
WITH rating AS(
    SELECT
        book_id,
        (MAX(session_rating) - MIN(session_rating)) AS rating_spread,
        ROUND(SUM(CASE WHEN session_rating <= 2 OR session_rating >= 4 THEN 1 ELSE 0 END)/COUNT(book_id),2) AS polarization_score
    FROM reading_sessions
    GROUP BY book_id
    HAVING (MIN(session_rating) <= 2 AND MAX(session_rating) >= 4)
        AND COUNT(book_id) >= 5
        AND polarization_score >= 0.6
)

SELECT r.book_id, b.title, b.author, b.genre, b.pages, r.rating_spread, r.polarization_score
FROM rating r
LEFT JOIN books b
    ON b.book_id = r.book_id
ORDER BY r.polarization_score DESC, b.title DESC

```

<hr>

### Excel Solution
