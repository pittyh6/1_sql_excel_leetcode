### Table

Table: books

| Column Name |  Type   |
| :---------- | :-----: |
| book_id     |   int   |
| title       | varchar |
| author      | varchar |
| genre       | varchar |
| pages       |   int   |

books =
| book_id | title | author | genre | pages |
| ------- | ---------------------- | ------------- | --------- | ----- |
| 1 | The Great Gatsby | F. Scott | Fiction | 180 |
| 2 | To Kill a Mockingbird | Harper Lee | Fiction | 281 |
| 3 | 1984 | George Orwell | Dystopian | 328 |
| 4 | Pride and Prejudice | Jane Austen | Romance | 432 |
| 5 | The Catcher in the Rye | J.D. Salinger | Fiction | 277 |

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

reading_sessions =
| session_id | book_id | reader_name | pages_read | session_rating |
| ---------- | ------- | ----------- | ---------- | -------------- |
| 1 | 1 | Alice | 50 | 5 |
| 2 | 1 | Bob | 60 | 1 |
| 3 | 1 | Carol | 40 | 4 |
| 4 | 1 | David | 30 | 2 |
| 5 | 1 | Emma | 45 | 5 |
| 6 | 2 | Frank | 80 | 4 |
| 7 | 2 | Grace | 70 | 4 |
| 8 | 2 | Henry | 90 | 5 |
| 9 | 2 | Ivy | 60 | 4 |
| 10 | 2 | Jack | 75 | 4 |
| 11 | 3 | Kate | 100 | 2 |
| 12 | 3 | Liam | 120 | 1 |
| 13 | 3 | Mia | 80 | 2 |
| 14 | 3 | Noah | 90 | 1 |
| 15 | 3 | Olivia | 110 | 4 |
| 16 | 3 | Paul | 95 | 5 |
| 17 | 4 | Quinn | 150 | 3 |
| 18 | 4 | Ruby | 140 | 3 |
| 19 | 5 | Sam | 80 | 1 |
| 20 | 5 | Tara | 70 | 2 |

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
Books
<img width="1020" height="472" alt="image" src="https://github.com/user-attachments/assets/e4d553be-b888-4f4f-abef-46c16e1314b3" />

Reading Session
<img width="928" height="736" alt="image" src="https://github.com/user-attachments/assets/a3c01bba-5215-466c-9b31-504d16576e40" />

Combined
<img width="1020" height="748" alt="image" src="https://github.com/user-attachments/assets/94c6cb5e-6553-4cc7-a34c-1d373749af8d" />

