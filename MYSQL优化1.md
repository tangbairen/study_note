##mysql查询语句分析，explain 用法
	explain  select * from user where id=10000

	+----+-------------+-------+-------+---------------+---------+---------+-------+------+-------+
	| id | select_type | table | type  | possible_keys | key     | key_len | ref   |rows | Extra |
	+----+-------------+-------+-------+---------------+---------+---------+-------+------+-------+
	|  1 | SIMPLE      | user  | const | PRIMARY       | PRIMARY | 4       | const |1 | NULL  |
	+----+-------------+-------+-------+---------------+---------+---------+-------+-------+-------+

	table：显示这一行的数据是关于哪张表的
	
	type：这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为const、eq_reg、ref、range、index和all
	
	possible_keys：显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从where语句中选择一个合适的语句
	
	key： 实际使用的索引。如果为null，则没有使用索引。很少的情况下，mysql会选择优化不足的索引。这种情况下，可以在select语句中使用use index（indexname）来强制使用一个索引或者用ignore index（indexname）来强制mysql忽略索引
	
	key_len：使用的索引的长度。在不损失精确性的情况下，长度越短越好
	
	ref：显示索引的哪一列被使用了，如果可能的话，是一个常数
	
	rows：mysql认为必须检查的用来返回请求数据的行数
	
	extra：关于mysql如何解析查询的额外信息。将在表4.3中讨论，但这里可以看到的坏的例子是using temporary和using filesort，意思mysql根本不能使用索引，结果是检索会很慢



##MySQL	批量更新

举个例子：

	UPDATE categories
	    SET display_order = CASE id
	        WHEN 1 THEN 3
	        WHEN 2 THEN 4
	        WHEN 3 THEN 5
	    END
	
	WHERE id IN (1,2,3)
	这句sql的意思是，更新display_order 字段：
	
	如果id=1 则display_order 的值为3，
	如果id=2 则 display_order 的值为4，
	如果id=3 则 display_order 的值为5。
	即是将条件语句写在了一起。
	
	这里的where部分不影响代码的执行，但是会提高sql执行的效率。
	
	确保sql语句仅执行需要修改的行数，这里只有3条数据进行更新，而where子句确保只有3行数据执行。

3.2 更新多值
	如果更新多个值的话，只需要稍加修改：
	
	UPDATE categories
	    SET display_order = CASE id
	        WHEN 1 THEN 3
	        WHEN 2 THEN 4
	        WHEN 3 THEN 5
	    END,
	    title = CASE id
	        WHEN 1 THEN 'New Title 1'
	        WHEN 2 THEN 'New Title 2'
	        WHEN 3 THEN 'New Title 3'
	    END
	WHERE id IN (1,2,3)

3.2 PHP实现
	这里以php为例，构造这条mysql语句：
	
	$display_order = array(
	    1 => 4,
	    2 => 1,
	    3 => 2,
	    4 => 3,
	    5 => 9,
	    6 => 5,
	    7 => 8,
	    8 => 9
	);
	
	$ids = implode(',', array_keys($display_order));
	
	$sql = "UPDATE categories SET display_order = CASE id ";
	foreach ($display_order as $id => $ordinal) {
	    $sql .= sprintf("WHEN %d THEN %d ", $id, $ordinal);
	}
	$sql .= "END WHERE id IN ($ids)";
	
	echo $sql;
	