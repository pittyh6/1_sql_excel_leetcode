### Problem: 626. Exchange Seats

<hr>

### Table

Seat table:

| id    |   student   |
| :---- | :---------: |
| 1     |    Abbot    |
| 2     |    Doris    |
| 3     |   Emerson   |
| 4     |    Green    |
| 5     |   Jeames    |
| :---- | :---------: |

Output:

| id    |   student   |
| :---- | :---------: |
| 1     |    Doris    |
| 2     |    Abbot    |
| 3     |    Green    |
| 4     |   Emerson   |
| 5     |   Jeames    |
| :---- | :---------: |

<hr>

### SQL Solution

1. First solution

```sql
--RunTime 67ms
SELECT s.id,
    CASE
        WHEN s.id%2=0 THEN (SELECT student FROM seat WHERE id = s.id-1 )
        WHEN s.id%2=1 AND s.id = (SELECT MAX(id) FROM seat) THEN (SELECT student FROM seat WHERE id = s.id)
        ELSE (SELECT student FROM seat WHERE id = s.id+1)
    END AS student
FROM seat s
ORDER BY s.id

```

<hr>

### Excel Solution
