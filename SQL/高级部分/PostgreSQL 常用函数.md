# PostgreSQL 常用函数

### 一、常用数学操作符
```sql
%    模      5 % 4      1
^    幂      2.0 ^ 3.0  8
|/   平方根  |/ 25.0     5
||/  立方根  ||/ 27.0    3
!    阶乘    5 !        120
!!   阶乘    !! 5       120
@    绝对值   @ -5.0     5

-- select 5 % 4 AS result;

```

### 二、常用数学函数

```sql
-- 取绝对值：abs()
select abs(-5); -- 5


-- 求余数：mod(x,y)
select mod(5,4); --1


-- 平方根：sqrt()
select sqrt(4); -- 2


-- 立方根：cbrt()
select cbrt(8); --2


-- 随机数：random(), 0.0 到 1.0 之间
select random(); -- 0.527410356327891


-- 不小于参数的最小的整数：cell()
select cell(-42.8); -- -42


-- 不大于参数的最大整数: floor()
select floor(-42.8); -- -43

```
