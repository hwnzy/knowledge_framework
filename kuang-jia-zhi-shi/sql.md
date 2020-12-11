# SQL

1. 导包

   ```text
    import pymysql
   ```

2. 创建连接对象

   ```text
    pymysql.connect(参数列表)
   ```

3. 获取游标对象

   ```text
    cursor =conn.cursor()
   ```

4. 执行SQL语句

   ```text
    row_count = cursor.execute(sql)
   ```

5. 获取查询结果集

   ```text
    result = cursor.fetchall()
   ```

6. 将修改操作提交到数据库

   ```text
    conn.commit()
   ```

7. 回滚数据

   ```text
    conn.rollback()
   ```

8. 关闭游标

   ```text
    cursor.close()
   ```

9. 关闭连接

   ```text
    conn.close()
   ```

