##  注：SQL 关键字不区分大小写  ##

### 两日期/时间之间相差的天数 ###
```sql
SELECT TO_DAYS(end_time) - TO_DAYS(start_time)
```

### 两日期/时间之间相差的十分秒数 ###
```sql
SELECT SEC_TO_TIME((UNIX_TIMESTAMP(end_time)) - UNIX_TIMESTAMP(start_time))
```

### 两日期/时间之间相差的秒数 ###
```sql
SELECT UNIX_TIMESTAMP(end_time) - UNIX_TIMESTAMP(start_time)
```

### 两日期之间差，X天Y小时Z分钟W秒 ###
```sql 
SELECT DATA_FORMATE(FROM_UNIXTIME(sum(UNIX_TIMESTAMP(end_time) - UNIX_TIMESTAMP(start_time))), '%天%时%分%秒')
```
