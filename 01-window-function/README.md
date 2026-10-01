# 01 窗口函数

## 核心语法

```sql
函数名() OVER (
    PARTITION BY 分组字段
    ORDER BY 排序字段
    [ROWS|RANGE BETWEEN 起点 AND 终点]
)
```

## 三类函数速查

| 类别 | 函数 | 用途 |
|---|---|---|
| 排名 | `ROW_NUMBER` `RANK` `DENSE_RANK` `NTILE` | 排名、去重、分组取前 N |
| 偏移 | `LAG` `LEAD` `FIRST_VALUE` `LAST_VALUE` | 环比、同比、前后行对比 |
| 聚合 | `SUM` `AVG` `COUNT` `MAX` `MIN` | 累计、移动平均、占比 |

## 必须记住的差异

`ROW_NUMBER` / `RANK` / `DENSE_RANK` 在值相等时的行为：

| 值 | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 100 | | | |
| 100 | | | |
| 90 | | | |

<!-- 自己造数据跑一遍，把结果填进上表 -->

## 题目记录

| # | 题目 | 思路关键词 | 状态 |
|---|---|---|---|
| 1 | | | ⬜ |
| 2 | | | ⬜ |

## 踩坑记录

<!-- -->