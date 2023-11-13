---
title: MySQL指令
categories: [MySQL , SQL]
tags: [MySQL , SQL]
date: 2023/04/20 17:04
updated: 2023/04/20 17:00
---

# MySQL常用指令

```bash
$ mysql -u <username> -p        # 登入
```

```sql
> SHOW DATABASES;               -- 顯示資料庫(預設橫式顯示)
> SHOW DATABASES \g             -- 橫式顯示
> SHOW DATABASES \G             -- 直式顯示
> CREATE DATABASE <name>;       -- 建立資料庫
> DROP DATABASE <name>;         -- 刪除資料庫
> USE <database>;               -- 使用資料庫
> SELECT DATABASE();            -- 查看目前所在資料庫
> SHOW TABLES;                  -- 顯示資料表
> DELIMITER <結束符號>          -- 更改結束符號
> SHOW WARNINGS;                -- 顯示錯誤訊息
> CREATE TABLE <name>           -- 建立資料表
(
    <column1> VARCHAR(20),
    <column2> INT
);
> SHOW COLUMNS FROM <table>;    -- 顯示資料表各欄位性質
> DESC <table>;                 -- 顯示資料表各欄位性質
> DROP TABLE <name>;            -- 刪除資料表
> SELECT * FROM <table>;        -- 顯示資料表中所有欄位資料
> SELECT <column> FROM <table>; -- 顯示資料表中欄位資料
> INSERT INTO <table>(<column1>, <column2>) -- 新增資料列
VALUES(<column1 data>, <column2 data>),
      (<column1 data>, <column2 data>);

```

# 資料庫階層架構

- 資料庫(database)
  - 資料表(table)
    - 欄位(column)

# 資料類型

## 數值類型

- 整數 (Integer)
  
  形式：`<data type> [UNSIGNED] [ZEROFILL]`

  |     類型      | Byte(s) |                    範圍                    |         UNSIGNED         |
  | :-----------: | :-----: | :----------------------------------------- | :----------------------- |
  |    TINYINT    |    1    | -128 ~ 127                                 | 0 ~ 255                  |
  |   SMALLINT    |    2    | -32768 ~ 32767                             | 0 ~ 65535                |
  |   MEDIUMINT   |    3    | -8388608 ~ 8388607                         | 0 ~ 16777215             |
  | INTEGER (INT) |    4    | -2147483648 ~ 2147483647                   | 0 ~ 4294967295           |
  |    BIGINT     |    8    | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446744073709551615 |

- 定點數 (Fixed-Point)
  - DECIMAL(M, D)(=DEC,NUMERIC,FIXED)
    - M：總共位數(最大65、預設30)；D：小數點後位數(最大30、預設0)
    - 小數點後不足補0，多的四捨五入
    - 可負數，不算位數

- 浮點數 (Floating-Point)
  - FLOAT(M, D)
    - 範圍：-3.402823466E+38 ~ -1.175494351E-38, 0, 1.175494351E-38 ~ 3.402823466E+38

- 雙精度浮點數
  - DOUBLE(M, D)(=DOUBLE PRECISION=REAL)
    - 範圍：-1.7976931348623157E+308 ~ -2.2250738585072014E-308, 0, 2.2250738585072014E-308 ~ 1.7976931348623157E+308

- 位 (Bit-Value)
  - BIT(M)
  - M：總共位數(最小1、最大64、預設1)
  - 儲存方式：b'1'
  - 顯示方式：
    - <column>+0 : 十進位
    - bin(<column>+0) : 二進位
    - oct(<column>+0) : 八進位
    - hex(<column>+0) : 十六進位

## 時間類型

- DATE
  - 日期：YYYY-MM-DD
  - 範圍：1000-01-01 ~ 9999-12-31
  - 可插入INT型，不需"-"

- TIME
  - 時間：HH:MM:SS 或 HH:MM 或 MMSS 或 SS
  - 範圍：-838:59:59 ~ 838:59:59
  - 可表示時間差
  - 可插入INT型，不需":"

- DATETIME
  - 日期時間：YYYY-MM-DD HH:MM:SS
  - 範圍：1000-01-01 00:00:00 ~ 9999-12-31 23:59:59

- YEAR
  - 年：YYYY 或 YY
  - 範圍：1901 ~ 2155
  - 可插入INT型或STRING型(2位或4位)
    - 1 ~ 69 或 "0" ~ "69" 表示 2001 ~ 2069
    - 70 ~ 99 或 "70" ~ "99" 表示 1970 ~ 1999
    - 0 表示 0000

- TIMESTAMP
  - 範圍 : 1970-01-01 00:00:01 UTC ~ 2038-01-19 03:14:07 UTC
  - 會隨著 timezone 改變
  - 預設當前時間
  - SELECT NOW()：查看系統當前時間
  ```sql
  > <column> TIMESTAMP
  > <column> TIMESTAMP DEFAULT NOW() ON UPDATE NOW()    -- 同上
  ```

- 設定時區
  - show variables LIKE "%time_zone%"：查看時區變量
  - SET time_zone = "+08:00"：更改時區為 UTC+8 小時
  - SET time_zone = "system"：更改時區為系統當前時區
  - time_zone 也可用國家名稱設置

# 過濾條件

形式：`SELECT * FROM <table> WHERE <conditions>;`

```sql
> SELECT * FROM <table> WHERE <column1>="XXX" <AND|OR> <NOT> <column2>="YYY";
> SELECT * FROM <table> WHERE <column1> IN ("XXX" , "YYY");     -- 等同於WHERE <column1>="XXX" OR <column2>="YYY"
> SELECT * FROM <table> WHERE <column1> BETWEEN "VAL1" AND "VAL2"
```

- 模糊搜尋（例如：顯示包含i的資料，不區分大小寫）
```sql
> SELECT * FROM <table> WHERE <column> LIKE "%i%";      -- %代表任意字符
> SELECT * FROM <table> WHERE <column> LIKE "_i_";      -- _代表1個任意字符
> SELECT * FROM <table> WHERE <column> LIKE "%\%i%";    -- 顯示包含%i的資料
```

# 新增資料表

形式：`CREATE TABLE(<column> <data type>);`

```sql
> CREATE TABLE IF NOT EXISTS <name>             -- 判斷資料表是否存在而建立
(
    <column> <data type>                        -- 資料預設為NULL
    <column> <data type> NOT NULL               -- 資料不可為NULL
    <column> DEFAULT <value>                    -- 資料預設值(預設為NULL)
    <column> <data type> PRIMARY KEY            -- 資料唯一性、不可重複、不可為NULL
    PRIMARY KEY(<column1>, <column2>)           -- 設置聯合PRIMARY KEY
    <column> <data type> UNIQUE                 -- 資料唯一性、不可重複、可為NULL
    <column> <data type> <PRIMARY KEY|UNIQUE> AUTO_INCREMENT   -- 未填值時自動遞增
);
```

# 取別名

```sql
> SELECT <column> AS <alias> FROM <table>;
```

# 更新及刪除資料

```sql
> UPDATE <table> SET <column> = "XXX" WHERE <conditions>;
> DELETE FROM <table> WHERE <conditions>;
```

# 字串處理函式

```sql
> SELECT CONCAT(<column1>, <column2>) AS <別名> FROM <table>;   -- 串接字串(<column1><column2>)
> SELECT CONCAT_WS("<間格符>", <column1>, <column2>) AS <別名> FROM <table>;    -- 串接字串(<column1><間格符><column2>)
> SELECT SUBSTRING("Hello World", 7);               -- 修剪字串(World)
> SELECT SUBSTRING("Hello World", -3);              -- 修剪字串(rld)
> SELECT SUBSTRING("Hello World", 1, 4);            -- 修剪字串(Hell)
> SELECT REPLACE("Hello World", "World", "MySQL");  -- 替換字串(Hello MySQL)
> SELECT REVERSE("Hello World");                    -- 反轉字串(dlroW olleH)
> SELECT LENGTH("Hello World");                     -- 字串長度(11)
> SELECT CHAR_LENGTH("Hello World");                -- 字串長度(11)
> SELECT CHARACTER_LENGTH("Hello World");           -- 字串長度(11)
> SELECT LOWER("Hello World");                      -- 轉小寫(hello world)
> SELECT UPPER("Hello World");                      -- 轉大寫(HELLO WORLD)
```

# 排序篩選

```sql
> SELECT * FROM <table> ORDER BY <column>;          -- 依照column升序排序
> SELECT * FROM <table> ORDER BY <column> DESC;     -- 依照column降序排序
> SELECT <column1>, <column2>, <column3> FROM <table> ORDER BY 3, 1;    -- 依照column3、column1升序排序
> SELECT * FROM <table> LIMIT 3;                    -- 顯示前3筆資料
> SELECT * FROM <table> LIMIT 2, 4;                 -- 顯示第2筆後4筆資料
```

# 資料統計

```sql
> SELECT COUNT(*) FROM <table>;                     -- 計算資料數量
> SELECT DISTINCT <column> FROM <table>;            -- 去除重複資料顯示唯一值
> SELECT COUNT(<column>) FROM <table> GROUP BY <column>;   -- 將column分組後算出每組的資料數量
> SELECT MIN(<column>) FROM <table>;                -- 計算column的最小值
> SELECT MAX(<column>) FROM <table>;                -- 計算column的最大值
> SELECT SUM(<column>) FROM <table>;                -- 計算column的和
> SELECT AVG(<column>) FROM <table>;                -- 計算column的平均值
> SELECT COUNT(<column>) FROM <table> GROUP BY <column> HAVING <column1> = "XXX";   -- 過濾GROUP BY後資料
```

# 更新欄位

```sql
> ALTER TABLE <name> ADD column3 INT PRIMARY KEY AUTO_INCREMENT;    -- 新增欄位
> ALTER TABLE <name> DROP column3;                                  -- 刪除欄位
> ALTER TABLE <name> CHANGE <column_old> <column_new> <data_type>;  -- 更改欄位名稱
> ALTER TABLE <name> MODIFY <column> <data_type>;                   -- 更改欄位性質
```


# 參考資料
1. https://hackmd.io/@kenny88881234/HJ_KDx1xS
2. https://dev.mysql.com/doc/refman/5.7/en/numeric-types.html