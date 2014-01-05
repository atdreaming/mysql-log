# 开始 #

	`show COLUMNS from 表名`   *列出字段*

	`show status`  *用于显示广泛的服务器状态信息*
	`show create databases show create table`  *分别显示穿件特定数据库活表的sql语句*
	`show grants`   *用来显示授予用户（所有用户或 特定用户）的安全权限*
	`show errors`; `show warnings`  *用来显示服务器错误或警告消息*
	`select DISTINCT 字段名 from 表名`   *显示字段中不同的行*
## 引擎 ##
	*查看引擎*	
	`show engines\G;`	
	`select engine from information_schema.engines where transactions=’yes’;`
	
	`alter table table_name add index index_name(field);`	*添加索引*
	`alter table table_name engine=innoDB;`	*更改引擎*
<b>当让数据表改用另一种存储引擎时，能否达到目的还要取决于新老两种存储引擎的功能是否兼容：</b>

<ul style="color:#666666">
<li>如果一个数据表包含这一个blob数据列，将不能把它转换为使用memory引擎。因为memory引擎不支持blob数据列。</li>
<li>如果有一个MyISAM数据表包含着fulltext或spatial索引，将不能把它转换为使用另一种引擎，因为只有MyISAM支持这两种索引。</li>
<li>Memory数据表存在于内存中，在服务器退出时将消失，因此，如果希望某个数据表的内容在服务器重新启动后仍然存在，就不应该把它转换为memory类型。</li>
<li>如果你使用了一个merge数据表来管理一组MyISAM数据表，就应该避免使用alert table语句去改变个别MyISAM数据表的结构，除非要决定对所有的成员MyISAM数据表和那个merge数据表做出同样的修改。Merge数据表的正常使用徐雅其全体成员MyISAM数据表有着同样的结构。</li>
<li>InnoDB数据表可以被转换为使用另一种存储引擎。如果对InnoDB数据表定义了外键约束条件，那些约束条件在转换后将不复存在，因为只有InnoDB才支持外键。</li>
</ul>
####删除索引####
	drop index index_name on table_name
####删除数据####
	delete from table_name where id>100;
	delete t1 from t1 inner join t2 on t1.id = t2.id;	从t1表把其id值可以在t2表里找到的行全部删掉
	delete t1, t2 from t1 inner join t2 on t1.id = t2.id;	删除t1,t2表中id相同的数据
	select t1.* from t1 left join t2 on t1.id=t2.id where t2.id is null;
#### 重新命名一个数据表 ####
	Alter table table_name rename to new_table_name;
	Rename table old_name to new_name;
	Rename table t1 to b1, t2 to b2, t3 to b3;
>
如果重新命名的某个MyISAM数据表是某个merge数据表的成员，你必须重新定义那个merge数据表，让它使用哪个MyISAM数据表的新名字。

## Where 子句操作符 ##
<table>
<tr>
<td>=</td><td>   等于</td>
</tr>
<tr>
<td><></td>  <td>不等于</td>
</tr><tr>
<td>!=</td>  <td>不等于</td></tr><tr>
<td>< </td>  <td>小于</td></tr><tr>
<td><=</td>  <td>小于等于</td></tr><tr>
<td>> </td>  <td>大于v</td></tr><tr>
<td>>=</td>  <td>大于等于</td></tr><tr>
<td>BETWEEN</td>  <td>在指定的两个值之间</td>
</tr></table>
#### IN 操作符 ####
	select 字段名 from 表名 where 字段名 IN (值1，值2，值3……);
	
	select 字段名 from 表名 where 字段名 NOT IN (值1，值2，值3……);

# Mysql正则 #
## 字符类 ##
<table style="font-size:12px"><tr><td>
[:alnum:]</td><td>  任意字母和数字（同 [a-zA-Z0-9]）</td></tr><tr><td>
[:alpha:]</td><td>   任意字符（同 [a-zA-Z]）</td></tr><tr><td>
[:blank:]</td><td>   空格和制表（同 [\\t]）</td></tr><tr><td>
[:cntrl:]</td><td>    ASCII控制字符（ASCII 0到31和127）</td></tr><tr><td>
[:digit:]</td><td>    任意数字（同 [0-9]）</td></tr><tr><td>
[:graph:]</td><td>   与[:print:]相同，但不包括空格</td></tr><tr><td>
[:lower:]</td><td>   任意小写字母（同 [a-z]）</td></tr><tr><td>
[:print:]</td><td>    任意可打印字符</td></tr><tr><td>
[:punct:]</td><td>   即不在 [:print:] 又不在 [:cntrl:] 中的任意字符</td></tr><tr><td>
[:space:]</td><td>   包括空格在内的任意空白字符（同 [\\f\\n\\r\\t\\v]）</td></tr><tr><td>
[:upper:]</td><td>   任意大写字母（同 [A-Z]）</td></tr><tr><td>
[:xdigit:]</td><td>    任意十六进制数字（同 [a-fA-f0-9])
</td></tr></table>

	\\    转义  匹配特殊字符 包括 `. | []` 以及迄今为止使用过的其他特殊字符
	      也可用来转义元字符   
	为了匹配特殊字符  \\  为前导
	\\\   匹配 \ 本身

####空白元字符####
<table><tr><td>
\\f  </td><td>   换页</td></tr><tr><td>
\\n  </td><td>   换行</td></tr><tr><td>
\\r  </td><td>   回车</td></tr><tr><td>
\\t  </td><td>   制表</td></tr><tr><td>
\\v  </td><td>   纵向制表</td></tr>
</table>
#### 基本字符匹配 ####
	select 字段名 from 表名 where 字段名 REGEXP ‘1000’ ORDER BY 字段名;
	查询字段中有 1000 的行

#### 重复元字符 ####
<table><tr><td>
*  </td><td>   0 个或多个匹配 </td></tr><tr><td>
+  </td><td>   1 个或多个匹配（等于 {1,}）</td></tr><tr><td>
?  </td><td>   0 个或1 个匹配（等于{0,1}）</td></tr><tr><td>
{n} </td><td>   指定数目的匹配</td></tr><tr><td>
{n,} </td><td>  不少于指定数目的匹配</td></tr><tr><td>
{n,m} </td><td> 匹配数目的范围 （m 不超过255）</td></tr>
</table>


#### 定位元字符 ####
<table><tr><td>
^   </td><td>   文本的开始 </td></tr><tr><td>
$   </td><td>   文本的结尾 </td></tr><tr><td>
[[:<:]] </td><td>  词的开始 </td></tr><tr><td>
[[:>:]] </td><td>  词的结尾 </td></tr>
</table>

## 拼接字符串 ##
	select  Concat(字段名, ‘（‘ , 字段名, ‘）’)  from 表名;
>
例：
Concat(vend_name, ‘（‘ , vend_cuntry, ‘）’)<br/>
结果：<br/>
ACME（USA）<br/>
Anvils R Us（USA）


# 算术操作符和mysql基本函数 #
## 去空格函数 ##
<table><tr><td>
RTrim() </td><td> 去掉值右边的所有空格 </td><tr></tr><td>
LTrim() </td><td> 去掉值左边的所有空格 </td><tr></tr><td>
Trim() </td><td>  去掉值左右两边的所有空格 </td>
<table>

## Mysql算术操作符 ##
<table><tr><td>
+  </td><td>   加 </td></tr><tr><td>
-  </td><td>   减 </td></tr><tr><td>
*  </td><td>   乘 </td></tr><tr><td>
/  </td><td>   除 </td></tr>
</table>
## 文本处理函数 ##
<table><tr><td>
Left()   </td><td>   返回串左边的字符 </td></tr><tr><td>
Length() </td><td>   返回串的长度 </td></tr><tr><td>
Locate()  </td><td>  找出串的子串 </td></tr><tr><td>
Lower()  </td><td>  将串转换为小写 </td></tr><tr><td>
LTrim()  </td><td>   去掉串左边的空格 </td></tr><tr><td>
Right()  </td><td>   返回串右边的字符 </td></tr><tr><td>
RTrim()  </td><td>   去掉串右边的空格 </td></tr><tr><td>
Soundex() </td><td>  返回串的SOUNDEX值 </td></tr><tr><td>
Substring() </td><td>  返回子串的字符 </td></tr><tr><td>
Upper()  </td><td>   将串转换为大写 </td></tr>
<table>
>
SOUNDEX 是将任何文本串转换为描述其语音表示的字母数字模式的算法，考虑了类似发音字符和音节，是的猛对串进行发音比较而不是字母比较<br/>
例：
Select cus_contact from customers where SOUNDEX(cust_contact) =SOUNDEX( ‘Y . lie’);<br/>
结果：<br>
Cust_contact<br/>
Y . lee

## Mysql日期函数 ##
<table><tr><td>
AddDate() </td><td>  增加一个日期（天、周等） </td></tr><tr><td>
AddTime() </td><td>  增加一个时间（时、分等） </td></tr><tr><td>
CurDate() </td><td>   返回当前日期 </td></tr><tr><td>
CurTime() </td><td>   返回当前时间 </td></tr><tr><td>
Date()   </td><td>    计算两个日期时间的日期部分 </td></tr><tr><td>
DateDiff()  </td><td>  计算两个日期之差 </td></tr><tr><td>
Date_Add() </td><td>  高度灵活的日期运算函数 </td></tr><tr><td>
Date_Fromat() </td><td> 返回一个格式化的日期或时间串 </td></tr><tr><td>
Day(date)    </td><td>    返回一个日期的天数部分 </td></tr><tr><td>
DayOfYear(date) </td><td>  返回一年中的第几天 </td></tr><tr><td>
DayOfMonth(date) </td><td> 返回月份中的日期 </td></tr><tr><td>
DayOfWeek(date) </td><td>  对于一个日期，返回对应的星期几1-7 </td></tr><tr><td>
WeekDay(date)  </td><td>  返回星期几 0-6 </td></tr><tr><td>
DayName(date) </td><td>  星期几的名字 英文 </td></tr><tr><td>
MonthName(date) </td><td> 月份名字  英文 </td></tr><tr><td>
Quarter(date) </td><td> 返回date一年中的季度  1-4 </td></tr><tr><td>
Hour(time) </td><td>  返回一个时间的小时部分 </td></tr><tr><td>
Minute(time) </td><td>  返回一个时间的分钟部分 </td></tr><tr><td>
Month(date) </td><td>  返回一个时间的月份部分 </td></tr><tr><td>
Now() </td><td>   返回一个当前日期和时间 </td></tr><tr><td>
Second(time) </td><td> 返回一个时间的秒部分 </td></tr><tr><td>
Time(date) </td><td>  返回一个日期时间的时间部分 </td></tr><tr><td>
Year(date)  </td><td>  返回一个日期的年份部分 </td></tr>
</talbe>

## 数值处理函数 ##
<table><tr><td>
Abs() </td><td>  返回一个函数的绝对值 </td></tr><tr><td>
Cos() </td><td>  返回一个角度的余弦 </td></tr><tr><td>
Exp() </td><td>  返回一个数的指数值 </td></tr><tr><td>
Mod() </td><td> 返回除操作的余数 </td></tr><tr><td>
Pi()  </td><td>   返回圆周率 </td></tr><tr><td>
Rand() </td><td>  返回一个随机数 </td></tr><tr><td>
Sin()  </td><td>   返回一个角度的正弦 </td></tr><tr><td>
Sqrt()  </td><td>  返回一个数的平方根 </td></tr><tr><td>
Tan()  </td><td>  返回一个角度的正切 </td></tr>
</table>
## SQL聚集函数 ##
<table><tr><td>
AVG()  </td><td>  返回某列的平均值  </td></tr><tr><td>
COUNT() </td><td> 返回某列的行数 </td></tr><tr><td>
MAX()  </td><td>  返回某列的最大值 </td></tr><tr><td>
MIN()  </td><td>  返回某列的最小值 </td></tr><tr><td>
SUM()  </td><td>  返回某列之和 </td></tr><tr><td>
COUNT( \* )  </td><td>  对表中行的数目进行计数，不管表中包含的是空值还是非空值  </td></tr><tr><td>
COUNT(column) </td><td> 对特定列中具有值的进行计数，忽略NULL值 </td></tr><tr><td>
MAX() </td><td> 在用于文本数据时，如果数据按相应的列排序，则MAX() 返回最后一行  </td></tr><tr><td>
MAX() </td><td> 忽略列值为NULL的行 </td></tr><tr><td>
MIN() </td><td>  在用于文本数据时，如果数据按相应的列排序，则MIN() 返回最前面的行 </td></tr><tr><td>
MIN() </td><td>  忽略列值为NULL的行 </td></tr><tr><td>
SUM() </td><td>  忽略列值为NULL 的行 </td></tr>
</table>
## 聚集不同值 ##
### GROUP BY ###
><ul>
<li>Group By 子句可以包含任意数目的列，这使的能对分组进行嵌套，为数据分组提供更细致的控制</li>
<li>如果Group By 子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）</li>
<li>Group By 子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在select中使用表达式，则必须在Group By 子句中指定相同的表达式。不能使用别名。</li>
<li>除聚集计算语句外，select 语句中的每个列都必须在Group By 子句中给出。</li>
<li>如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。</li>
<li>Group By 子句必须出现在where子句之后，Order By 子句之前。</li>
<ul>


*使用WITH ROLLUP关键字，可以得到每个分组以及每个分组汇总级别（针对每个分组）的值，如下所示：*

	select vend_id, count(*) as num_prods FROM products GROUP BY vend_id WITH ROLLUP;







<table><tr><th>
ORDER BY </th><th> GROUP BY </th></tr><tr><td>
排序产生的输出 </td><td>
任意列都可以使用（甚至非选择的列也可以使用）</td></tr><tr><td>
不一定需要  </td><td>  分组行。但输出可能不是分组的顺序</td></tr><tr><td>
只可能使用选择列或表达式列，而且必须使用每个选择列表达式。</td><td>
如果与聚集函数一起使用列（或表达式），则必须使用</td></tr>
</table>
>
 一般在使用GROUP BY 子句时，应该也给出ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖GROUP BY 排序数据


## SELECT子句及其顺序 ##
<table><tr><th> 
子句  </th><th>	说明</th><th>	是否必须使用 </th></tr><tr><td>
SElECT</td><td>	要返回的列或表达式	</td><td>是</td></tr><tr><td>
FROM</td><td>	从中检索数据的表</td><td>	尽在重表选择数据时使用</td></tr><tr><td>
WHERE</td><td>	行级过滤</td><td>	否</td></tr><tr><td>
GROUP BY</td><td>	分组说明</td><td>	仅在按组计算聚集时使用</td></tr><tr><td>
HAVING</td><td>	组级过滤</td><td>	否</td></tr><tr><td>
ORDER BY</td><td>	输出排序顺序</td><td>	否</td></tr><tr><td>
LIMIT</td><td>	要检索的行数</td><td>	否</td></tr>
</table>

## Delete 语句 ##

	`Delete from table`  *删除表中所有数据*
	`Delete` *只删除表中的数据 但不删除表*

	*删除表中所有数据另一种方式*
	`Truncate table;`  
	`truncate` *实际上是删除原来的表并重新创建一个表，并不是逐行删除数据*

## 视图 ##
### 使用视图 ###
* 视图用create view语句来创建
* 使用show create view viewname；来查看创建视图的语句
* 用drop删除视图，其语法为 drop view viewname;
* 更新视图时，可以先用drop再用create，也可以直接用 create or replace view。如果要更新的视图不存在，则第二条语句会创建一个视图；如果要跟新的视图存在；则第二条语句会替换原有视图。

	
	`create view 视图名 as select * from talbename;`
	

### 小结 ###
* 视图是可以更新的（即可以对它们使用insert、update和delete）。更新一个视图将更新其基表，视图本身没有数据，如果对视图增加或删除行，实际上是对其基表增加或删除行。
* 但是，并非所有的视图都是可以更新的。基本上可以说，如果mysql不能正确的确定被更新的基数据，则不允许更新（包括插入和删除）。这实际上意味着，如果视图定义中有以下操作，则不能进行视图的更新：
	1. 分组（使用GROUP BY 和 HAVING）；
	1. 联结；
	1. 子查询；
	1. 并；
	1. 聚集函数（min()、count()、sum()等）；
	1. DESTINCT；
	1. 导出（计算）列。
* 视图的主要用于数据的检索（SELECT语句），而不用于更新（insert、update、delete）；


## 存储过程 ##
### 临时更改命令行实用程序的语句分隔符： ### 
	DELIMITER //; 
	DELIMITER ;

### 创建存储过程: ###
	DELIMITER //
	create procedure 名称()
		DEGIN
			……;
	END
	DELIMITER ;

### 使用存储过程 ###
	CALL 名称()；
### 删除存储过程 ###
	DROP PROCEDURE 名称；  名称后面没有括号
	也可以使用 drop procedure of exists 名称；

### 带参数的存储过程 ###
	Delimiter %%
	create procedure price(
		out p1 decimal(8,2),
		out p2 decimal(8,2),
		out p3 decimal(8,2),
	)
	Begin
		Select min(price) into p1 from product;
		Select max(price) into p2 from product;
		Select avg(price) into p3 from product;
	End%%
	Delimiter ;


#### 调用 ####   
	call price(@p1,@p2,@p3);
	select @p1;
	select @p2;
	select @p3;
### 检查存储过程 ###
	show create procedure 名称；
	show procedure status  获得时间，创建等详细信息的存储过程列表  列出存储过程
	show procedure status like ‘aaa’ 使用like指定一个过滤模式


## 游标 ##
>
游标是一个存储在Mysql服务器上的数据库查询，它不是一条SELECT语句，而是被该语句检索出来的结果集，在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。使用游标可以在检索出来的行中前进或后退一行或多行。

1. 在能够使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用SELECT语句。
1. 一旦声明后，必须打开游标以供使用。这个过程用前面定义的SELECT语句把数据实际检索出来。
1. 对于填有数据的游标，根据需要取出（检索）各行。
1. 在结束游标使用时，必须关闭游标。

### 创建游标 ###
<p>游标用DECLARE语句创建。DECLARE命名游标，并定义相应的SELECT语句，根据需要带WHERE和其他子句。</p>
	Create procedure lookform()
	Begin
		Declare goodsprice cursor for
		Select goodprice from goods;
	End;
	该游标在存储过程处理完成后，游标就消失（因为它局限与存储过程）；




### 打开和关闭游标 ###
	OPEN goodsprice; 在处理open语句时执行查询，存储检索出的数据以供浏览器和滚动
	CLOSE goodsprice;  close释放游标使用的所有内部内存和资源，因此在每个游标不再需要时都应该关闭
