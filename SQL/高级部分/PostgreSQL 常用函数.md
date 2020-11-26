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


-- 不小于参数的最小的整数：ceil()
select ceil(-42.8); -- -42


-- 不大于参数的最大整数: floor()
select floor(-42.8); -- -43

```


### 三、字符串函数

```sql
-- 字符串连接： string || string
select 'ruby' || 'china' ; -- rubychina


-- 字符串长度： length()
select length('i love you') ; -- 10


-- 字符串转小写： lower()
select lower('RUBY'); -- ruby


-- 字符串转大写： upper()
select upper('rails'); -- RAILS


-- 字符串每个单词的第一个子母转为大写： initcap()
select initcap('ruby on rails'); -- Ruby on Rails


-- 字符串替换： 
-- overlay(string placing string from int [for int])
-- 和
-- replace(string text, from text, to text)
select overlay('Ruby_______Rails' placing ' on ' from 5 for 7); -- Ruby on Rails
select replace('Ruby_______Rails', '_______', ' on '); -- Ruby on Rails


-- 截取字符串： 
-- substring(string [from int] [for int]) 
-- 和 
-- substr(string, from [, count])
select substring('Ruby on Rails' from 1 for 4); -- Ruby
select substring('Ruby on Rails', 1, 4);        -- Ruby
select substr('Ruby on Rails', 1, 4);           -- Ruby
select substr('Ruby on Rails', 6);              -- on Rails
select substring('foobar' from '%#"o_b#"%' FOR '#'); -- oob  (这里#是转义符，双引号内的模式是返回部分)


-- 子字符串位置 position(substring in string)
select position('-' in 'ruby-china'); -- 5 (利用这个特性可以用于判断字符串中是否包含某个子字符串)


-- 去除首尾空格：trim(string)
select trim('    ruby    ');  -- 'ruby'


-- 去除左边空格/字符： ltrim(string text [, characters text])
select ltrim('    ruby'); -- ruby
select ltrim('__ruby','_'); -- ruby


-- 去除右边空格/字符： rtrim(string text [, characters text])
select rtrim('ruby    ');    -- ruby
select rtrim('ruby__','_');  -- ruby


-- 获取字符串的 MD5 值：md5()
select md5('ruby'); -- 58e53d1324eef6265fdb97b08ed9aadf


-- 字符串分割： split_part(string text, delimiter text, field int)
select split_part('i||love||you', '||', 2); -- love

```


