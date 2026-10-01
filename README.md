# sql-practice

数据方向 SQL 专项练习。**区别于业务 SQL：这里重点是窗口函数、连续性问题、分组 TopN。**

## 进度

| 主题 | 目标题数 | 已完成 |
|---|---|---|
| 基础（SELECT / JOIN / GROUP BY） | 12 | 0 |
| 子查询 / HAVING | 8 | 0 |
| **窗口函数** | **15** | **0** |
| 连续性 / 留存 / 环比 | 10 | 0 |
| 数仓建模练习 | 5 | 0 |
| **合计** | **50** | **0** |

## 目录

| 目录 | 内容 |
|---|---|
| `01-window-function/` | 窗口函数：排名、偏移、聚合 |
| `02-consecutive-login/` | 连续登录 / 连续 N 天 |
| `03-topn-per-group/` | 每组 TopN |
| `04-retention-ratio/` | 留存率、环比同比 |
| `05-warehouse-modeling/` | 拉链表、SCD、分层 |
| `notes/` | 概念笔记 |

## 每道题的写法

```sql
-- 题目：查询每个用户连续登录的天数
-- 知识点：ROW_NUMBER + 日期减法分组
--
-- 思路：
--   1. 用 ROW_NUMBER() 给每个用户的登录日期编号
--   2. 用 login_date - rn 得到"分组标识"（连续日期减等差数列 = 常数）
--   3. 按 用户 + 分组标识 GROUP BY，COUNT(*) 就是连续天数
--
-- 关键点：为什么 login_date - rn 是常数？
--   因为连续日期的差值恰好是等差数列，做差后必然相等。
--
-- 易错点：要去重（同一天可能登录多次）

SELECT user_id, COUNT(*) AS consecutive_days
FROM (
    SELECT user_id, login_date,
           login_date - INTERVAL ROW_NUMBER() OVER (
               PARTITION BY user_id ORDER BY login_date
           ) DAY AS grp
    FROM (SELECT DISTINCT user_id, login_date FROM login_log) t
) x
GROUP BY user_id, grp;
```

## 笔记

- [数仓分层与建模](./notes/warehouse-layers.md)

## 我的薄弱点

<!-- 每周更新 -->

- 例：`ROWS BETWEEN` 和 `RANGE BETWEEN` 的差异还没吃透