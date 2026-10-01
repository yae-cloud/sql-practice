# 02 连续性问题

> 连续登录、连续 N 天活跃、连续上涨 —— 这是数仓 SQL 最经典的题型。

## 核心套路：**日期 - 行号 = 分组标识**

原理：连续日期是等差数列（公差 1 天），减去同样公差为 1 的行号序列，差值必然恒定。

```
login_date   rn    date - rn
2026-01-01    1    2025-12-31   ┐
2026-01-02    2    2025-12-31   ├ 同一组 = 连续
2026-01-03    3    2025-12-31   ┘
2026-01-07    4    2026-01-03   ← 断开了，新组
```

## 模板

```sql
WITH t AS (
    SELECT DISTINCT user_id, login_date
    FROM login_log
),
numbered AS (
    SELECT user_id, login_date,
           DATE_SUB(login_date, INTERVAL
               ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) DAY) AS grp
    FROM t
)
SELECT user_id, MIN(login_date) AS start_date,
       MAX(login_date) AS end_date,
       COUNT(*) AS consecutive_days
FROM numbered
GROUP BY user_id, grp;
```

## 变体

| 变体 | 改动 |
|---|---|
| 连续 N 天以上 | 加 `HAVING COUNT(*) >= N` |
| 最大连续天数 | 外层再套 `MAX(consecutive_days)` |
| 连续上涨（非日期） | 用 `value - rn` 或 `value - rn * 步长` |

## 易错点

- ⚠️ 必须先 `DISTINCT`，同一天多次登录会破坏连续性
- ⚠️ 日期类型不同数据库减法写法不同（`DATE_SUB` / `date - rn` / `dateadd`）

## 我的题目记录

| # | 题目 | 状态 |
|---|---|---|
| 1 | | ⬜ |

## 踩坑记录

<!-- -->