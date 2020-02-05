![image-20200204122913763](/Users/dylan/Library/Application Support/typora-user-images/image-20200204122913763.png)

### 项目经验

![image-20200204123018481](/Users/dylan/Library/Application Support/typora-user-images/image-20200204123018481.png)

![image-20200204123042525](/Users/dylan/Library/Application Support/typora-user-images/image-20200204123042525.png)

### 鲁棒性很重要

### 不要怕问问题

![image-20200204150521998](/Users/dylan/Library/Application Support/typora-user-images/image-20200204150521998.png)

# 题目

## 基础知识

1. 复制运算符函数

2. 数组中重复的数字(0~n-1)

   [排序法]

   [哈希表]

   [比较下标和数字本身法]如果相等就下一个,不相等就交换

    ```java
   public bool duplicate(int nums[],int length){
   	//robust
     int cnt = 0;
     for(int i = 0 ; i < length ; i++){
       while(nums[i] != i ){
         if(nums[i] == nums[nums[i]]) return false;
   		}
       swap(nums[i],nums[nums[i]])
   	}
   }
    ```

   这一次不能修改

   ```java
   public int getDuplication(int [] nums){
     int l = 1;
     int r = nums.length-1;
     while(l<=r){
       int m = ((r-l)>>1) + l;
       int count = countNums(nums,l,m);//用来计算数组中从l到m之间
       if(l == r){
         if(count > 1) return l;
         else break;
   		}
     }
   }
   ```

4. 二维数组中的查找(String)

   每一行从左到右,每一列从上到下递增

   ![image-20200204141909091](/Users/dylan/Library/Application Support/typora-user-images/image-20200204141909091.png)

   从右上角搜寻

   ![image-20200204141956218](/Users/dylan/Library/Application Support/typora-user-images/image-20200204141956218.png)

5. 替换空格,把每个空格换成%20

   每次发现空格都增加字符数组的长度时间复杂度太低了

   先统计空格的个数然后增加数组的长度,然后遍历把%20插进去

6. 从尾到头打印链表

   [栈]

   [递归]

7. 前序+中序重建二叉树

8. 二叉树的下一个节点

   如果有右子树,下一个就是右子树的最左边的节点

   如果没有就找父节点,直到当前节点是父节点的左子树

9. 用两个栈实现队列,pop和add

   Stack1 & stack2, 往stack1里面倒,如果要弹出先把stack1的倒入stack2,然后弹出stack2中的,插入的时候往stack1里面插入

   总结:如果stack2非空,直接弹出stack2中的,否则把stack1倒入stack2然后再弹出stack2的,始终插入到stack1当中

10. 斐波那契数列:保存到数组(或者临时变量里面)就完事儿

    青蛙跳台阶问题:和上面一样

    青蛙跳台阶扩展:可以跳1,2,...,n级,![image-20200204150257282](/Users/dylan/Library/Application Support/typora-user-images/image-20200204150257282.png)

    矩形覆盖方法 f(8) = f(7) + f(6)

11. 旋转数组的最小数字

    判断中间元素和末尾的大小,如果中间小于后面往左边找,如果中间大于后面就往右边找

    注意一开始的mid要初始化为0,这样才能保证没有旋转的情况也能过

    ![image-20200204152405776](/Users/dylan/Library/Application Support/typora-user-images/image-20200204152405776.png)

12. 矩阵中的路径(找到出现了string的路径)----回溯

13. 机器人的运动范围(有些格子可以进,有些不行)----回溯

14. 剪绳子长度为n剪成m段,使得这些小绳子的乘积最大

    - [dp] product[i] = max(product[j] * products[i-j],product[i]);

      如果小于2返回0,如果等于2返回1,如果等于3返回2(2*1)

      ![image-20200204154249810](/Users/dylan/Library/Application Support/typora-user-images/image-20200204154249810.png)

    - 贪心

      ![image-20200204154730860](/Users/dylan/Library/Application Support/typora-user-images/image-20200204154730860.png)

15. 二进制中1的个数

    - 不断右移判断最右边的数是不是1,但是如果是负数,一直移位还是要保证最高位为1,最后变成0xffffffff而陷入死循环
    - 把1左移判断
    - ![image-20200204175803559](/Users/dylan/Library/Application Support/typora-user-images/image-20200204175803559.png)

## 代码的完整性

16. 整数次方

    - 最简单的办法

    - 简单办法+判断指数为负数/0,负数就先算正的再求倒数,0就报错(可以提一下三种方式的优缺点再和面试官讨论)

    - ![image-20200204180957854](/Users/dylan/Library/Application Support/typora-user-images/image-20200204180957854.png)

      ![image-20200204181246883](/Users/dylan/Library/Application Support/typora-user-images/image-20200204181246883.png)

![image-20200204181307556](/Users/dylan/Library/Application Support/typora-user-images/image-20200204181307556.png)

17. 打印从1到最大的n位数

    - 可能超过int范围,不能用int

    - 用字符数组表示数

      ```java
      public void print(int n){
        if(n <= 0) return;
        int [] nums = new int[n+1];
        arrays.fill(nums,'\0');
        nums[n] = '\0';
        while(!Increment(nums)){
      		print;//只有遇到非0的时候才开始打印
        }
      }
      
      public boolean Increment(int [] nums){
        boolean isOverflow = false;
        int takeOver = 0;
        int length = nums.length;
        for(int i = length - 1; i >= 0;i--){
         	int sum = nums[i]-'0'+ takeOver;
          if(i == length - 1) sum++;// 最后一位的话,需要+1
          if(sum < 10){//不需要向前进位了
            nums[i] = '0' + sum;
            break;
          }
          else{
      			if(i == 0) isOverflow = true;
            else {
      				sum -= 10;
              takeOver = 1;
              nums[i] = '0' + sum;
            }
          }
      	}
      }
      ```

    - 全排列

      ![image-20200204185814740](/Users/dylan/Library/Application Support/typora-user-images/image-20200204185814740.png)

18. - 删除链表的节点
      1⃣️要删除的是尾节点,一直遍历到最后,把倒数第二个节点的next设置为null
      2⃣️链表只有一个节点,删除之后还要令head=null
      3⃣️正常情况,要删除i,把i的下一个节点j的值复制到i的地方上,再令i的下一个指向j的下一个

    - 删除链表中重复的节点

      ```java
      public ListNode deleteDuplicates(ListNode head) {
              if(head == null) return null;
              if(head.next == null) return head;
              ListNode dummy = new ListNode(0);
              dummy.next = head;
              ListNode p = dummy;
              while(p.next.next!=null){
                  if(p.next.val == p.next.next.val){    
                      ListNode q = p.next.next;
                      while(q!=null&&q.val == p.next.val)
                          q = q.next;
                      if(q == null) {
                          p.next = q;
                          return dummy.next;
                      }
                      else p.next = q;
                  }
                  else p = p.next;
              }
              return dummy.next;
          }
      ```

      ```java
      public void deleteDuplication(ListNode head){
      	if(head == null) return;
        ListNode pre = null;
        ListNode node = head;
        while(node != null){
          ListNode next = node.next;
          if((next != null) && (next.value == node.value)){
      			ListNode delete = node;
            while(delete != null && delete.value == node.value)
              delete = delete.next;
          }
          if(pre == null) head = next;
          else pre.next = next;
          node = next;
        }
      
      ```

19. 正则表达式匹配 . *,有 * 的时候有三种情况,无*的时候直接匹配

    ```java
    public boolean match(String str,String pattern){
      if(str.length == 0 && pattern.length == 0) return true;
      if(str.length != 0 && pattern.length == 0) return false;
      char [] p  = pattern.toArray();
      char [] str = str.toArray();
      if(pattern.charAt(1) == '*'){
        if(pattern.charAt(0) == str.charAt(0)||pattern.charAt(0)=='.'&& str.length!=0)
          return 
          //这里用c++写,java太长了,比如aaa和b*ac*a
         	match(str+1,patten+2) 			// *匹配一个字符  (aa,ac*a)
         	|| match(str+1,pattern)			// *匹配多个字符	(aa,b*ac*a)
          || match(str,pattern+2);		// *匹配0个字符	 (aaa,ac*a)
        else return match(str,pattern+2);
      }
      if(*str==*pattern||(*pattern=='.'&&*str!='\0'))
        return match(str+1,pattern+1);
      return false;
    }
    ```

20. 表示数值的字符串

    ..

    ![image-20200205091208726](/Users/dylan/Library/Application Support/typora-user-images/image-20200205091208726.png)

    ![image-20200205091222009](/Users/dylan/Library/Application Support/typora-user-images/image-20200205091222009.png)

    ![image-20200205091233707](/Users/dylan/Library/Application Support/typora-user-images/image-20200205091233707.png)

    ![image-20200205091242991](/Users/dylan/Library/Application Support/typora-user-images/image-20200205091242991.png)

    

21. 调整数组顺序使奇数位于偶数前面

    ```java
    public void reorder(int [] data){
    	int length = data.length();
      if(length == 0 || data == null) return;
      int l = 0,r = length - 1;
      while(l < r){
        while(l < r && (data[l] & 1) == 0) l++;
        while(l < r && (data[r] & 1) == 1) r--;
        if(l < r) swap(data[l],data[r]);
      }
    }
    ```

    - 负数放在非负数的前面,改一下就好
    - 能被三整除的都放在不能被三整除的前面

    最后发现是要把判断条件整合到一个函数里面,解耦合

### 代码的鲁棒性

22. 链表中倒数第k个节点

    两个指针

    注意点:

    - head为空指针
    - 总节点少于k
    - k为0

    ![image-20200205093652258](/Users/dylan/Library/Application Support/typora-user-images/image-20200205093652258.png)

23. 链表中环的入口点

    一个快一个慢,相遇点之后再走k步即可

    ![image-20200205100920769](/Users/dylan/Library/Application Support/typora-user-images/image-20200205100920769.png)

24. 反转链表

    ```java
    public ListNode reverse(ListNode head){
      ListNode newHead = null,node = head,pre = null;
      while(node != null){
    		ListNode next = node.next;
        if(next == null) newHead = node;
       	node.next = pre;
        pre = node;
        node = next;
      }
      return newHead;
    }
    ```

25. 合并两个排序的链表

    ```java
    public ListNode merge(ListNode a,ListNodeb){
    	if(a == null) return b;
      if(b == null) return a;
      ListNode newHead = null;
      if(a.value < b.value){
        newHead = a;
        newHead.next = merge(a.next,b);
      }
      else{
    		newHead = b;
        newHead.next = merge(a,b.next);
      }
    	return newHead;
    }
    ```

26. 树的子结构

    ```java
    public boolean isChild(TreeNode son,TreeNode dad){
      if(same(son,dad)) return true;
      return same(son,dad.left)||same(son,dad.right);
    }
    public boolean same(TreeNode a,TreeNode b){
    	if(a == null && b ==null) return true;
      else if(a == null || b == null) return false;
      if(a.val == b.val)
        return same(a.left,b.left)&&same(a.right,b.right);
      else return false;
    }
    ```

### 解决面试题的思路

27. 二叉树的镜像

    ```java
    public void mirror(TreeNode root){
    	if(root == null) return ;
      if(root.left == null && root.right == null) return ;
      root.left = mirror(root.left);
      root.right = mirrot(root.right);
      TreeNode tmp = root.left;
      root.left = root.right;
      root.rigth = tmp;
      return;
    }
    ```

28. 对称的二叉树

    ```java
    public boolean symmertric(TreeNode root){
      return symmertric(root.left,root.right);
    }
    public boolean symmertric(TreeNode a,TreeNode b){
      if(a == null && b == null) return true;
      if(a == null || b == null) return false;
      if(a.value != b.value) return false;
      return symmertric(a.left,b.right)
        && symmertric(a.right,b.left);
    }
    ```

29. 顺时针打印矩阵

    ```java
    				int up = 0; // 赋值上下左右边界
            int down = matrix.length - 1;
            int left = 0;
            int right = matrix[0].length - 1;
            while (true) {
                for (int i = left; i <= right; ++i)
                    ans.add(matrix[up][i]); // 向右移动直到最右
                if (++up > down)  break; 
                for (int i = up; i <= down; ++i)
                    ans.add(matrix[i][right]); // 向下
                if (--right < left) break; // 重新设定有边界
                for (int i = right; i >= left; --i)
                    ans.add(matrix[down][i]); // 向左
                if (--down < up) break; // 重新设定下边界
                for (int i = down; i >= up; --i)
                    ans.add(matrix[i][left]); // 向上
                if (++left > right) break; // 重新设定左边界
            }
    ```

### 举例让抽象问题具体化

30. 包含min函数的栈

    用一个min栈来保存当前的最小值

    ```java
    public class minStack<T>(){
    	Stack<T> data = new Stack<>();
      Stack<T> min = new Stack<>();
      public void push(T value){
        data.push(value);
        if(min.size() == 0 || value < min.top()) m.push(value);
        else m.push(m.top());
      }
      public T pop(){
    		try{
          data.pop();
          min.pop();
        }
      }
      public T min(){
    		try{
         	return min.top();
        }
      }
    }
    ```

31. 栈的压入、弹出序列

    ```java
    public boolean isPopOrder(int [] push,int[] pop){
      //12345 45321
            if (pushA==null||popA==null) return false;
            int index=0;      //表示输出数组的索引
            Stack<Integer> sta=new Stack<>();
            for (int i = 0;i < pushA.length;i++ ){
              /*
              12345 45321
              sta
              1
              12
              123
              1234 45321
              123  5321
              1235 5321
              123  321
              12   21
              1    1
              x    x
              
              */
                sta.push(pushA[i]);
                while (!sta.isEmpty()&&sta.peek()==popA[index]){
                    sta.pop();
                    index++;
                }
            }
            return sta.empty();
        }
    }
    ```

32. - 从上到下打印二叉树

      ```java
      public void print(TreeNode root){
        if(root == null) return;
        Queue<Integer> queue = LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
          TreeNode node = queue.peek();
          queue.pop();
          System.out.println(node);
          if(node.left!=null) queue.offer(node.left);
          if(node.right != null) queue.offer(node.right);
        }
      }
      ```

    - 分行从上到下打印二叉树

      ```java
      while(!queue.isEmpty()){
        int size = queue.size();
        while(size -- ){
      		TreeNode node = queue.peek();
          queue.pop();
          System.out.println(node);
          if(node.left!=null) queue.offer(node.left);
          if(node.right != null) queue.offer(node.right);
        }
        syso();
      }
      ```

    - 之字形打印二叉树

      ![image-20200205132544275](/Users/dylan/Library/Application Support/typora-user-images/image-20200205132544275.png)

      ![image-20200205132555163](/Users/dylan/Library/Application Support/typora-user-images/image-20200205132555163.png)

      ```java
      void Print(TreeNode root){
      	if(root == null) return;
        Stack<TreeNode> levels[2];
        int current = 0;
        int next = 1;
        levels[current].push(root);
        while(!levels[0].empty()||!levels[1].empty()){
      		TreeNode node = levels[current].top();
          levels[current].pop();
          print(node.value);
          if(current == 0){
      			if(node.left != null) levels[next].push(node.left);
            if(node.right != null) levels[next].pusj(node.right);
          }
          else{
            if(node.left != null) levels[next].push(node.right);
            if(node.rigth != null) levels[next].push(node.left);
          }
          if(levels[current].empty()){
            syso();
            current = 1 - current;
            next = 1 - next;
          }
        }
        
      }
      ```

33. 二叉搜索树的后续遍历序列

    找到那个分界点,判断是不是分界点右边都小于根节点

    然后分别判断分界点左边右边是不是一个后续遍历序列

    ```java
    public boolean sequence(int [] nums){
      int length == nums.length;
      if(length == 0) return false;
      int root = nums[length - 1];
      int i =0;
      for(i < length -1;i++)
     		if(nums[i] < root) break;
      int j = i;
      for(;j < length - 1; j++){
        if(nums[j] < root) return false;
      }
      boolean left = i > 0 ? sequence(nums,i):true;
      boolean right = i < length - 1 ? sequence(nums+i,length - i - 1);
      return (left && rigth);
    }
    ```

34. 二叉树中和为某一值的路径

    ```java
    public void path(TreeNode node , int target,Stack<int> stack){
    	if(node == null || target < 0) return;
      if(node.val > target) return;
      if(node.left == null&&node.right == null&& target == 0) {
        ans.add(stack);
        return;
      }
      stack.add(node);
      if(node.left != null) path(node.left,target-node.val,stack);
      if(node.right != null) path(node.rigth,target-node.val,stack);
      stack.pop();
    }
    ```

### 分解让复杂问题简单化

35. 复杂链表的复制

    - 复制原始链表的节点跟在原来的后面
    - 复制节点的sibling指向原来的sibling
    - 分成两个链表

    ```java
    public void clone1(ListNode head){
      ListNode node = head;
      while(node != null){
       	ListNode newNode = new ListNode;
        newNode.val = node.val;
        newNode.next = node.next;
        node.next = newNode;
        node = newNode.next;
      }
    }
    public void copySibling(ListNode head){
      ListNode node = head;
      while(node != null){
        ListNode next = node.next;
        next.sibling = node.sibling;
        node = next.next;
      }
    }
    
    public void reconnect(ListNode head){
      Listnode node = head;
      ListNode cloneHead = null;
      ListNode clone = null;
      while(node != null){
        ListNode clone = node.next;
        node.next = clone.next;
        node = node.next;
      }
    }
    ```

36. 二叉搜索树与双向链表

    

    ```java
    public TreeNode convert(TreeNode root){
      
    }
    public TreeNode convertNode(TreeNode node,TreeNode last){
      if(node == null) return ;
      TreeNode pre = convertNode(node.left,last);
      node.left = last;
      if(last != null) last.right = node;
      last = node;
      if(node.right != null) convertNode(node.right,last);
      
    }
    ```

37. 序列化二叉树

    ![image-20200205171934885](/Users/dylan/Library/Application Support/typora-user-images/image-20200205171934885.png)

38. 字符串的排列

    ```java
    public List<String> permutation(String string){
    	char [] chars = string.toCharArray();
      permutation(chars,0);
    }
    public void permutation(char [] chars,int begin){
    	if(begin == chars.length - 1) {
    		list.add (chars);
        return;
      }
     	for(int i = begin;i < length;i++){
    		swap(i,j);
        permutation(chars,begin+1);
        swap(i,j);
      }
    }
    ```

    - p199扩展题

### 优化时间和空间效率

39. 数组中出现次数超过一半的数字

    - partition函数,如果选中的数字下标是n/2,那么这个数就是,如果>n/2往左边找,否则往右边着
- 保存数字和次数,相同+1,不同-1,如果为0,保存下一个数字,这个数字就是
    
40. 最小的k个数

    - partition函数,如果下标是k-1就停下来,不然往左往右继续找(会修改输入的数组
    - 最大堆,set,如果多了就删掉一个放进去,如果不够就直接放进去(不修改数组,适合海量数据)

    ![image-20200205182121639](/Users/dylan/Library/Application Support/typora-user-images/image-20200205182121639.png)

41. 数据流中的中位数

    ![image-20200205183427159](/Users/dylan/Library/Application Support/typora-user-images/image-20200205183427159.png)

    ![image-20200205183438526](/Users/dylan/Library/Application Support/typora-user-images/image-20200205183438526.png)

    左边最大堆右边最小堆,总数是偶数时插入最小堆,奇数时插入最大堆

    插入最小堆时,先把新数据插入最大堆,再把最大堆中的最大数插入最小堆

    //插入最大堆时,先把新数据插入最小堆,再把最小堆中的最小数插入最大堆

42. 连续子数组的最大和

    ```java
    public int max(int [] nums){
    	if(nums == null || nums.length == 0)  return 0;
      int length = nums.length;
      int [] dp = new int[length];
      dp[0] == nums[0];
      int max = Integer.MIN_VALUE;
      for(int i = 1; i < length; i++){
    		dp[i] = dp[i-1]>0? dp[i-1]+nums[i]:nums[i];
        max = Math.max(max,dp[i]);
      }
      return max;
    }
    ```

43. 1~n整数中1出现的次数

    21345 => 

    1~1345:   

    1346~21345 最高位:10000~19999 共10000个数

    1346~11345 & 11346~21345   

    ```java
    public int NumberOfOne(String string){
    	if(string == null || string.length == 0) return 0;
      int first = string.charAt[0]-'0';
      if(length == 1 && first == 0) return 0;
      if(length == 1 && first > 0) return 1;
      int numFirstDigit = 0;
      if(first > 1) numFirstDigit = PowerBase10(length -1);
      else if(first == 1) numFirstDigit = atoi(string + 1) + 1;
      //1346~21345除了最高位 4*9
      int numOtherDigits = first*(length - 1) * PowerBase10(length - 2);
      //1~1345
      int numRecursive = NumberOfOne(string+1)
    }
    ```

    1~21345

    - 1~1345
    - 1346~21345:     2 * 4 *
      - 1346~11345  4 * 10 * 
      - 11346~21345

    

44. 数字序列中某一位的数字

    





















### 代码的完整性

#### 测试需要考虑的方面:==功能测试、边界测试、负面测试==

1. 功能测试:完成基本的功能
2. 边界测试:是否能用于大数,循环,递归
3. 负面测试:不符合要求的时候作出错误处理

#### 三种错误处理的方法

![image-20200204180618849](/Users/dylan/Library/Application Support/typora-user-images/image-20200204180618849.png)







#### 1.复制运算符函数



#### 3.数组中重复的数字