# Date时间格式

## 字符串与date格式的转换

### 转换字符串为日期

使用ORACLE内部函数to_date（）

变量定义如下：

string_value ：为字符串直接值（字符串本身）、字符串列（数据库中定义的某个表的某列）或某字符串内部函数的返回值。

date_format为合法的Oracle日期格式。

格式例子：

```
to_date('2004-05-07 13:23:44','yyyy-mm-dd hh24:mi:ss')
```

### 转换日期为字符串

变量定义如下：

使用ORACLE内部函数to_char（）

date_value ：为日期型直接值（日期本身）、日期型列值（数据库中定义的某个表的某列）或某内部函数的返回的日期型值。

date_format为合法的Oracle日期格式。

### oracle时间计算

sysdate+1 加一天

sysdate+1/24 加1小时

sysdate+1/(24*60) 加1分钟

sysdate+1/(24*60*60) 加1秒钟

类推至毫秒0.001秒

加法

select sysdate,add_months(sysdate,12) from dual; –加1年

select sysdate,add_months(sysdate,1) from dual; –加1月

select sysdate,to_char(sysdate+7,'yyyy-mm-dd HH24:MI:SS') from dual; –加1星期

select sysdate,to_char(sysdate+1,'yyyy-mm-dd HH24:MI:SS') from dual; –加1天

select sysdate,to_char(sysdate+1/24,'yyyy-mm-dd HH24:MI:SS') from dual; –加1小时

select sysdate,to_char(sysdate+1/24/60,'yyyy-mm-dd HH24:MI:SS') from dual; –加1分钟

select sysdate,to_char(sysdate+1/24/60/60,'yyyy-mm-dd HH24:MI:SS') from dual; –加1秒

减法

select sysdate,add_months(sysdate,-12) from dual; –减1年

select sysdate,add_months(sysdate,-1) from dual; –减1月

select sysdate,to_char(sysdate-7,'yyyy-mm-dd HH24:MI:SS') from dual; –减1星期

select sysdate,to_char(sysdate-1,'yyyy-mm-dd HH24:MI:SS') from dual; –减1天

select sysdate,to_char(sysdate-1/24,'yyyy-mm-dd HH24:MI:SS') from dual; –减1小时

select sysdate,to_char(sysdate-1/24/60,'yyyy-mm-dd HH24:MI:SS') from dual; –减1分钟

select sysdate,to_char(sysdate-1/24/60/60,'yyyy-mm-dd HH24:MI:SS') from dual; –减1秒