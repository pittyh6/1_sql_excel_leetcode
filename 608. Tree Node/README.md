### Table

Input:
Tree table:
| id | p_id |
|:----|:------:|
| 1 | null |
| 2 | 1 |
| 3 | 1 |
| 4 | 2 |
| 5 | 2 |

Output:

| id    |   type   |
| :---- | :------: |
| 1     |   Root   |
| 2     |  Inner   |
| 3     |   Leaf   |
| 4     |   Leaf   |
| 5     |   Leaf   |
| :---- | :------: |

<hr>

### Problem: 608. Tree Node

Table: Tree

| Column Name | Type |
| :---------- | :--: |
| id          | int  |
| p_id        | int  |

id is the column with unique values for this table.
Each row of this table contains information about the id of a node and the id of its parent node in a tree.
The given structure is always a valid tree.

Each node in the tree can be one of three types:

"Leaf": if the node is a leaf node.
"Root": if the node is the root of the tree.
"Inner": If the node is neither a leaf node nor a root node.

<hr>

### SQL Solution

1. First solution
   --Runtime: 116 ms

```sql
SELECT id,
    CASE
        WHEN p_id IS NULL THEN 'Root'
        WHEN id IN (SELECT p_id FROM tree) AND p_id IS NOT NULL THEN 'Inner'
        ELSE 'Leaf'
    END AS Type
FROM Tree
```

<hr>

### Excel Solution
