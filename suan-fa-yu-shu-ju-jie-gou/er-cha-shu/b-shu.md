# B树

###  m阶B树

1. 每个结点至多拥有m棵子树
2. 根结点至少拥有两颗子树（存在子树的情况下）
3. 除了根结点以外，其余每个分支结点至少拥有 m/2 棵子树
4. 所有的叶结点都在同一层上
5. 有 k 棵子树的分支结点则存在 k-1 个关键码，关键码按照递增次序进行排列
6. 关键字数量需要满足m/2-1（向上取整） &lt;= n &lt;= m-1

### 插入

![](../../.gitbook/assets/image%20%2870%29.png)

![](../../.gitbook/assets/image%20%2856%29.png)

![](../../.gitbook/assets/image%20%2886%29.png)

### 删除

![](../../.gitbook/assets/image%20%2874%29.png)

![](../../.gitbook/assets/image%20%282%29.png)

![](../../.gitbook/assets/image%20%2882%29.png)

