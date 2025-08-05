 ## Table
 Activity
 
| Column Name  | Type    |
| :------------|:--------|
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |



# Problem: 550. Game Play Analysis IV

 Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to determine the number of players who logged in on the day immediately following their initial login, and divide it by the number of total players.

 ## SQL Solution
 ```sql
WITH first_date AS (
    SELECT player_id, 
    MIN(event_date) AS prev_day
    FROM Activity
    GROUP BY player_id
), next_date AS (
    SELECT a.player_id 
    FROM Activity a
    JOIN first_date f
        ON f.player_id = a.player_id
        AND a.event_date = DATE_ADD(f.prev_day, INTERVAL 1 DAY)
)

SELECT ROUND(
    COUNT(DISTINCT n.player_id)* 1.0 / COUNT(DISTINCT f.player_id), 2 
)AS fraction
FROM first_date f
LEFT JOIN next_date n
    ON n.player_id = f.player_id
```

## Excel Solution
<img width="558" height="192" alt="image" src="https://github.com/user-attachments/assets/3f7af89f-1034-4b98-acb6-5f8f2111cc70" />
<img width="610" height="268" alt="image" src="https://github.com/user-attachments/assets/efb8ff09-5175-4833-af92-9b1d9a3526a6" />
<img width="579" height="246" alt="image" src="https://github.com/user-attachments/assets/dbd014fa-0b19-4489-ad04-b22c12613713" />
<img width="1072" height="368" alt="image" src="https://github.com/user-attachments/assets/f688cb9e-8e81-4b72-98b9-ed7a712c03d5" />

