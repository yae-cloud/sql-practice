# 03 每组 TopN

## 三种写法

### 写法一：`ROW_NUMBER()`（推荐，最通用）

```sql
SELECT * FROM (
    SELECT category, product, sales,
           ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) AS rn
    FROM sales_table
) t
WHERE rn <= 3;
```

### 写法二：`RANK()` / `DENSE_RANK()`（允许并列）

- 需要"并列都取"时用 `RANK` 或 `DENSE_RANK`
- 只取固定条数时用 `ROW_NUMBER`

### 写法三：关联子查询（不推荐，但面试可能问）

```sql
SELECT * FROM sales_table a
WHERE (SELECT COUNT(*) FROM sales_table b
       WHERE b.category = a.category AND b.sales > a.sales) < 3;
```

## 三者取舍

| 写法 | 并列时行为 | 性能 | 适用 |
|---|---|---|---|
| `ROW_NUMBER` | 任意取一个 | 好 | 固定条数 |
| `RANK` | 并列后跳号（1,1,3） | 好 | 并列都取 |
| `DENSE_RANK` | 并列后不跳号（1,1,2） | 好 | 并列都取且名次连续 |
| 子查询 | 并列都取 | 差 | 仅作了解 |

## 我的题目记录

| # | 题目 | 用的写法 | 状态 |
|---|---|---|---|
| 1 | | | ⬜ |

## 踩坑记录

<!-- -->