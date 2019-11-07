# LeetCode 题解

## 算法题

### 简单

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/two-sum
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

一遍哈希，键值对为（数组值，索引）。遍历数组，如果target-nums[i]存在于哈希中，则返回这个值的索引和i；不存在哈希中则插入。

- 时间复杂度On，空间On

##### 代码

```c++

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> map;
        int size=nums.size();
        vector<int> result;
        for(int i=0;i<size;i++){
            int c=target-nums[i];
            if(map.count(c)>0){
                result.push_back(map[c]);
                result.push_back(i);
                return result;
            }
            map[nums[i]]=i;
        }
        
        return result;
    }
};
```







#### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

> 通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下**六种情况：**
>
> - I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
>
> - X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
> - C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
>
> 给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/roman-to-integer
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

将负数部分先减去，只剩下正数部分相加即可。

- 负数部分是比后续元素值小的部分，需要减去
- 利用栈，依次压栈，将比当前元素值小的部分弹栈并减去并将当前元素压栈。最后将栈内剩余元素加起来减去负值部分就是答案。
- 复杂度，时间On，空间On

##### 代码

```c++
class Solution {
public:
    int romanToInt(string s) {
        map<char,int> m;
        m['I']=1;
        m['V']=5;
        m['X']=10;
        m['L']=50;
        m['C']=100;
        m['D']=500;
        m['M']=1000;
        int r=0;
        stack<int> st;
        for(int i=0;i<s.size();i++){
            if(i==0){
                st.push(m[s[i]]);
                continue;
            }
            while(!st.empty()&&st.top()<m[s[i]]){
                r-=st.top();
                st.pop();
            }
            st.push(m[s[i]]);
        }
        while(!st.empty()){
            r+=st.top();
            st.pop();
        }
        return r;
    }
};
```



#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

##### 思路

使用栈，拿到当前元素，如果当前元素和栈顶元素匹配则弹出，否则把当前元素压栈，直到遍历完。如果此时栈不为空则无效，为空则有效。

- 时间复杂度On，空间On

##### 代码

```c++
class Solution {
public:
    bool isValid(string s) {
        if(!s.size()) return true;
        map<char,char> m;
        m['(']=')';
        m['[']=']';
        m['{']='}';
        stack<char> st;
        for(int i=0;i<s.size();i++){
            if(st.empty()){
                st.push(s[i]);
                continue;
            }
            char top=st.top();
            if(m[top]==s[i]) st.pop();
            else st.push(s[i]);
        }
        
        return st.empty();
    }
};
```



#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

> 将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

##### 思路

数组**升序。**类似于合并有序数组，使用2+1指针，两个指针分别指向两个链表未被合并部分的最小元素，一个指针指向当前已经合并的最大元素。

- 时间复杂度O(m+n)，空间复杂度O1

##### 代码

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* cur1=l1,*cur2=l2,*head=NULL,*cur=NULL,*tmp;
        if(cur1&&cur2){
            if(cur1->val>cur2->val){
                head=cur=l2;
                cur2=cur2->next;
            }else{
                head=cur=l1;
                cur1=cur1->next;
            }
            //more than 0 is NULL
        }else if(cur1){
            return cur1;
        }else{
            return cur2;
        }
        while(cur1&&cur2){
            if(cur1->val<cur2->val){
                cur->next=cur1;
                cur1=cur1->next;
            }else{
                cur->next=cur2;
                cur2=cur2->next;
            }
            cur=cur->next;
            
        }
        
        if(cur1) cur->next=cur1;
        if(cur2) cur->next=cur2;
        return head;
    }
};
```







#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

> 给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

更为有效的方式（代码2）是使用双指针法，如下代码1虽使用了双指针，但进行了删除操作，时间上会增加。

- 时间复杂度On，空间O1

##### 代码1

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        auto i=nums.begin();
        while(i!=nums.end()){
            auto j=i+1;
            int count=0;
            while(j!=nums.end()&&*j==*i){
                j++;
                count++;
            }
            if(count>0){
                nums.erase(i+1,j);
            }
            i++;
        }
        return nums.size();
    }
};
```

##### 代码2

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int i=0;
        for(int j=1;j<nums.size();j++){
            if(nums[i]!=nums[j]){
                i++;
                nums[i]=nums[j];
            }
        }
        return i+1;
    }
};
```



#### [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

> 给定一个数组 *nums* 和一个值 *val*，你需要**原地**移除所有数值等于 *val* 的元素，返回移除后数组的新长度。

##### 思路

使用双指针，一个指向当前元素，一个指向被删除后前缀数组的最后元素。

- 时间复杂度On，空间O1

##### 代码

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int l=0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]!=val){
                nums[l]=nums[i];
                l++;
            }
        }
        return l;
    }
};
```





#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)

> 给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
>
> 最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
>
> 你可以假设除了整数 0 之外，这个整数不会以零开头。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/plus-one
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

仅需要处理进一的情况，还有溢出的情况。

- 复杂度：时间On，空间O1

##### 代码

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        digits[digits.size()-1]++;
        int i=digits.size()-1;
        while(digits[i]==10){
            digits[i]=0;
            i--;
            if(i<0) break;
            digits[i]++;
        }
        if(i<0) digits.insert(digits.begin(),1);
        return digits;
    }
};
```





#### [67. 二进制求和](https://leetcode-cn.com/problems/add-binary/)

> 给定两个二进制字符串，返回他们的和（用二进制表示）。
>
> 输入为**非空**字符串且只包含数字 `1` 和 `0`。

##### 思路

从低位向高位加，注意进位

- 时间复杂度O(max(m,n))，空闲复杂度O(max(m,n))

##### 代码

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int i=a.size()-1,j=b.size()-1;
        string result="";
        int carry=0;
        while(i>=0&&j>=0){
            if(a[i]=='1'&&b[j]=='1'){
                result.insert(0,carry?"1":"0");
                carry=1;
            }else if(a[i]=='1'||b[j]=='1'){
                if(carry==1){
                    result.insert(0,"0");
                }else{
                    result.insert(0,"1");
                }
            }else{
                result.insert(0,carry?"1":"0");
                carry=0;
            }
            i--;
            j--;
        }
        while(i>=0){
            if(a[i]=='1'){
                if(carry==1){
                    result.insert(0,"0");
                }else{
                    result.insert(0,"1");
                }
            }else{
                result.insert(0,carry?"1":"0");
                carry=0;
            }
            i--;
        }
        while(j>=0){
            if(b[j]=='1'){
                if(carry==1){
                    result.insert(0,"0");
                }else{
                    result.insert(0,"1");
                }
            }else{
                result.insert(0,carry?"1":"0");
                carry=0;
            }
            j--;
        }
        if(carry==1) result.insert(0,"1");
        return result;
    }
};
```



#### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

> 实现 int sqrt(int x) 函数。
>
> 计算并返回 x 的平方根，其中 x 是非负整数。
>
> 由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/sqrtx
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

利用二分查找（还有牛顿法，没整，感觉logn挺好了），除了开始的几个数外，总有$ \sqrt{x}<=\frac{x}{2} $。右边界从这开始找就行了。

- 时间复杂度Ologn，空间O1

##### 代码

```c++
class Solution {
public:
    int mySqrt(int x) {
        
        int left=0,right=x;
        if(x>4) right=x/2;
        while(left<=right){
            long mid=left+(right-left)/2;
            long s=mid*mid;
            long s1=(mid+1)*(mid+1);
            if(s>x) right=mid-1;
            else if(s<=x&&s1>x) return mid;
            else left=mid+1;
        }
        return -1;
    }
};
```



#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

> 给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。
>
> **说明:**
>
> - 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
>
> - 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/merge-sorted-array
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

和合并两个有序链表类似，使用2+1指针，2个指针分别指向两个数组的最小待合并元素，1个指针指向已合并元素的最后位置。

- 时间复杂度O(m+n)，空间复杂度O(m+n)

##### 代码

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if(!nums2.size()) return;
        if(!nums1.size()){
            nums1=nums2;
            return;
        }
        vector<int> r;
        int i=0,j=0;
        while(i<m&&j<n){
            if(nums1[i]<nums2[j]){
                r.push_back(nums1[i]);
                i++;
            }else{
                r.push_back(nums2[j]);
                j++;
            }
        }
        while(i<m){
                r.push_back(nums1[i]);
                i++;
        }
        while(j<n){
                r.push_back(nums2[j]);
                j++;
        }
        nums1=r;

    }
};
```



#### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

>给定一个二叉树，找出其最大深度。
>
>二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
>
>**说明**: 叶子节点是指没有子节点的节点。
>
>
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

使用递归实现，二叉树的最大深度就是1+左右子树的最大深度。

- 时间复杂度On，空间复杂度On



##### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        return 1+max(maxDepth(root->left),maxDepth(root->right));
    }
    int max(int left,int right){
        return left>right?left:right;
    }
};
```



#### [107. 二叉树的层次遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

> 给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

##### 思路

简单粗暴，层次遍历然后reverse。

- 时间复杂度On，空间On

##### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        vector<vector<int>> result;
        if(!root) return result;
        while(!q.empty()){
            int size=q.size();
            vector<int> v;
            for(int i=0;i<size;i++){
                TreeNode*cur=q.front();
                v.push_back(cur->val);
                q.pop();
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
            }
            result.push_back(v);
        }
        int i=0,j=result.size()-1;
        while(i<j){
            auto tmp=result[i];
            result[i]=result[j];
            result[j]=tmp;
            i++;
            j--;
        }
        return result;
    }
};
```





#### [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

> 给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
>
> 说明：本题中，我们将空字符串定义为有效的回文串。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/valid-palindrome
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

利用双指针，分别从前往后找和从后往前找，如果当前都是字母则比较，不同则false，如果都相同则true

- 时间复杂度On，空间复杂度O1

##### 代码

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        if(s.size()<=1) return true;
        int left=0,right=s.size()-1;
        transform(s.begin(),s.end(),s.begin(),::tolower);
        while(left<right){
            while(left<s.size()&&!isAlphaDigit(s[left])) left++;
            while(right>=0&&!isAlphaDigit(s[right])) right--;
            if(left>=s.size()||right<0) break;
            if(s[left]!=s[right]) return false;
            left++;
            right--;
        }
        return true;
    }
    bool isAlphaDigit(char c){
        return (c>='0'&&c<='9')||(c>='a'&&c<='z');
    }
};
```



#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

> 给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/rotate-array
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

利用额外数组，将前面部分移动到后面，再将前面的清除。空间复杂度是On，对应**代码1**

- 时间复杂度On，空间复杂度On

还有更高效的方法就是利用翻转，不使用额外空间。对应**代码2**

```
原始数组                  : 1 2 3 4 5 6 7
反转所有数字后             : 7 6 5 4 3 2 1
反转前 k 个数字后          : 5 6 7 4 3 2 1
反转后 n-k 个数字后        : 5 6 7 1 2 3 4 --> 结果
```

- 时间复杂度On，空间复杂度O1

##### 代码1

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k=k%nums.size();
        if(k==0) return;
        int size=nums.size();
        for(int i=0;i<size-k;i++){
            nums.push_back(nums[i]);
        }
        nums.erase(nums.begin(),nums.begin()+size-k);
    }
};
```

##### 代码2

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k=k%nums.size();
        if(k==0) return;
        reverse(nums.begin(),nums.end());
        reverse(nums.begin(),nums.begin()+k);
        reverse(nums.begin()+k,nums.end());
    }
};
```





#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
>
>
> 说明:
>
> 必须在原数组上操作，不能拷贝额外的数组。
> 尽量减少操作次数。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/move-zeroes
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

利用双指针，一个指向“新”数组的当前元素，一个指向“旧”数组的非零元素。最后将新数组末尾全部置0即可。

- 时间复杂度On，空间复杂度O1



##### 代码

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int size=nums.size();
        int i=0,j=0;
        for(;j<size;j++){
            if(nums[j]!=0){
                nums[i++]=nums[j];
            }
        }
        while(i<size){
            nums[i]=0;
            i++;
        }
    }
};
```



#### [557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

> 给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
>
>
> 注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/reverse-words-in-a-string-iii
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

按照单词来翻转，不复杂，直接上。

- 时间复杂度On，空间O1

##### 代码

```c++
class Solution {
public:
    string reverseWords(string s) {
        int i=0,j=0;
        while(i<s.size()){
            while(j<s.size()&&s[j]!=' ') j++;
            int r=j-1;
            while(i<r){
                char tmp=s[i];
                s[i]=s[r];
                s[r]=tmp;
                i++;
                r--;
            }
            i=j+1;
            j=i;
        }
        return s;
    }
};
```



#### [724. 寻找数组的中心索引](https://leetcode-cn.com/problems/find-pivot-index/)

> 给定一个整数类型的数组 nums，请编写一个能够返回数组“中心索引”的方法。
>
> 我们是这样定义数组中心索引的：数组中心索引的左侧所有元素相加的和等于右侧所有元素相加的和。
>
> 如果数组不存在中心索引，那么我们应该返回 -1。如果数组有多个中心索引，那么我们应该返回最靠近左边的那一个。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/find-pivot-index
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

先排除数组为空的情况。求和减去第一个元素，表示第一个元素右边的和，左边的和初始化为0，依次右移修改sum值直到两者相等或者中心索引超出数组的最大索引

- 时间复杂度On，空间O1

##### 代码

```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        if(nums.size()==0) return -1;
        int sum_right=0;
        for_each(nums.begin(),nums.end(),[&](int n){
            sum_right+=n;
        });
        sum_right-=nums[0];
        int sum_left=0;
        int i=0;
        while(i+1<nums.size()&&sum_left!=sum_right){
            sum_left+=nums[i];
            sum_right-=nums[i+1];
            i++;
        }
        return sum_left==sum_right?i:-1;
    }
};
```



#### [747. 至少是其他数字两倍的最大数](https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others/)

> 在一个给定的数组nums中，总是存在一个最大元素 。
>
> 查找数组中的最大元素是否至少是数组中每个其他数字的两倍。
>
> 如果是，则返回最大元素的索引，否则返回-1。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思略

首先如果这个最大数存在的话就肯定是整个数组的最大值。那么思路就是找到这个最大值，并且比较其他元素是否满足条件即可。

- 时间复杂度On，空间O1

##### 代码

```c++
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        if(nums.size()==0) return -1;
        int index=0,max=nums[0];
        for(int i=1;i<nums.size();i++){
            if(nums[index]<nums[i]){
                max=nums[i];
                index=i;
            }
        }
        
        for(int i=0;i<nums.size();i++){
            if(index==i) continue;
            if(nums[i]*2>max) return -1;
        }
        return index;
    }
};
```





#### [1249. 移除无效的括号](https://leetcode-cn.com/problems/minimum-remove-to-make-valid-parentheses/)

> 给你一个由 '('、')' 和小写字母组成的字符串 s。
>
> 你需要从字符串中删除最少数目的 '(' 或者 ')' （可以删除任意位置的括号)，使得剩下的「括号字符串」有效。
>
> 请返回任意一个合法字符串。
>
> 有效「括号字符串」应当符合以下 **任意一条** 要求：
>
> - 空字符串或只包含小写字母的字符串
> - 可以被写作 AB（A 连接 B）的字符串，其中 A 和 B 都是有效「括号字符串」
> - 可以被写作 (A) 的字符串，其中 A 是一个有效的「括号字符串」
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/minimum-remove-to-make-valid-parentheses
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

和检测括号有效性相同，需要记录每个括号的位置。最后在残留在栈内的就是无效的括号，去除即可。

- 时间复杂度On，空间复杂度On

##### 代码

```c++
struct Node{
    char c;
    int i;
};
class Solution {
public:
    string minRemoveToMakeValid(string str) {
        stack<Node> s;
        for(int i=0;i<str.size();i++){
            if(str[i]!='('&&str[i]!=')') continue;
            if(str[i]=='('){
                s.push({str[i],i});
                continue;
            }
            if(s.empty()){
                s.push({str[i],i});
                continue;
            }
            if(s.top().c=='('){
                s.pop();
                continue;
            }
            s.push({str[i],i});
        }
        while(!s.empty()){
            
            str.erase(s.top().i,1);
            s.pop();
        }
        return str;
    }
};
```

### 中等



#### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

>  给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
>
> **示例:**
>
> 输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
> 输出:
> [
>   ["ate","eat","tea"],
>   ["nat","tan"],
>   ["bat"]
> ]
> **说明：**
>
> 所有输入均为小写字母。
> 不考虑答案输出的顺序。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/group-anagrams
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

首先想到哈希。因为这是一种映射，将字母异位词映射到同一个组，哈希函数就是字符串排序结果。

- 时间复杂度On，空间复杂度On（也可以认为空间是结果的一部分可以看做和不同字母异位词的个数相关）

##### 代码

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> result;
        unordered_map<string,vector<string>> map;
        for(int i=0;i<strs.size();i++){
            string tmp=strs[i];
            sort(tmp.begin(),tmp.end());
            map[tmp].push_back(strs[i]);
        }
        for(auto it=map.begin();it!=map.end();it++){
            result.push_back(it->second);
        }
        return result;
    }
};
```



#### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

> 给定一个二叉树，返回它的*中序* 遍历。

##### 思路

二叉树的前中后序遍历都是深度优先搜索，前序是第一次访问时便记录，中序是第二次访问，后序是第三次访问。我们可以使用自定义栈来模拟函数栈。

- 时间复杂度On，空间复杂度On

##### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        set<TreeNode*> visited;
        stack<TreeNode*> s;
        vector<int> result;
        if(!root) return result;
        s.push(root);
        while(!s.empty()){
            TreeNode* cur=s.top();
            s.pop();
            if(!cur) continue;
            if(visited.find(cur)==visited.end()){
                visited.insert(cur);
                s.push(cur->right);
                s.push(cur);
                s.push(cur->left);
            }
            else{
                result.push_back(cur->val);
            }
        }
        return result;
    }
};
```



#### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。
>
> 假设一个二叉搜索树具有**如下特征**：
>
> - 节点的左子树只包含小于当前节点的数。
>
> - 节点的右子树只包含大于当前节点的数。
> - 所有左子树和右子树自身必须也是二叉搜索树。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/validate-binary-search-tree
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

利用中序遍历有序，如果中序遍历存在当前元素比上一个元素小（假设升序）则不是二叉搜索树，否则遍历完成则是二叉搜索树。

- 时间复杂度On，空间复杂度On

##### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> s;
        set<TreeNode*> visited;
        s.push(root);
        int last,first=1;
        while(!s.empty()){
            TreeNode* cur=s.top();
            s.pop();
            if(!cur) continue;
            if(!visited.count(cur)){
                visited.insert(cur);
                s.push(cur->right);
                s.push(cur);
                s.push(cur->left);
            }else{
                if(first==1){
                    first=0;
                    last=cur->val;
                    continue;
                }
                if(last>=cur->val) return false;
                last=cur->val;
            }
        }
        return true;
    }
};
```



#### [102. 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。
>
>
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

树的层次遍历就是一种广度优先搜索，使用队列来完成操作。

- 时间复杂度On，空间复杂度On

##### 代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;        
        queue<TreeNode*> q;
        if(!root) return result;
        q.push(root);
        while(!q.empty()){
            
            int size=q.size();
            vector<int> curlevel;
            for(int i=0;i<size;i++){
                TreeNode* cur=q.front();
                curlevel.push_back(cur->val);
                if(cur->left)
                    q.push(cur->left);
                if(cur->right)
                    q.push(cur->right);
                q.pop();
            }
            result.push_back(curlevel);
            
            
        }
        return result;
        
    }
};
```





#### [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

> 给定一个字符串，逐个翻转字符串中的每个单词。
>
>  说明：
>
> 无空格字符构成一个单词。
> 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
> 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/reverse-words-in-a-string
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

整体思路是首先翻转所有字符，然后再按单词翻转一次，得到答案。注意去除空格。

- 时间复杂度On（不考虑string erase造成的浪费），空间复杂度O1

##### 代码

```c++
class Solution {
public:
    string reverseWords(string s) {
        //reverse all chars
        reverse(s.begin(),s.end());
        //remove space at start&end
        while(s[0]==' ') s.erase(0,1);
        while(s[s.size()-1]==' ') s.erase(s.size()-1,1);
        //reverse all word in turn
        int i=0,j=0;
        while(i<s.size()){
            while(j<s.size()&&s[j]!=' ') j++;
            //remove extra space
            if(s[j]==' '){
                int k=j+1;
                while(k<s.size()&&s[k]==' ') s.erase(k,1);
            }
            int r=j-1;
            //cout<<i<<r<<endl;
            while(i<r){
                char tmp=s[i];
                s[i]=s[r];
                s[r]=tmp;
                i++;
                r--;
            }
            i=j+1;
            j=i;
        }
        return s;
        
        
    }
};
```



#### [498. 对角线遍历](https://leetcode-cn.com/problems/diagonal-traverse/)

> 二维数组对角线遍历。
>
> ![img](pics/diagonal_traverse.png)



##### 思路

> x+y=b(b=0,1,2,3)

##### 代码

```c++
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        //x+y=b(b=0,1,2)
        vector<int> result;
        if(matrix.size()==0) return result;
        int M=matrix.size();
        int N=matrix[0].size();
        int max_len=M>N?M:N;
        int flag=0;//0->up,1->down
        for(int b=0;b<=2*max_len-2;b++){
            if(!flag){
                int y=b;
                while(y>=M) y--;
                for(;y>=0;y--){
                    if(b-y>=N) continue;
                    result.push_back(matrix[y][b-y]);
                }
                
            }else{
                int x=b;
                while(x>=N) x--;
                for(;x>=0;x--){
                    if(b-x>=M) continue;
                    result.push_back(matrix[b-x][x]);
                }
            }
            flag=!flag;
        }
        return result;
    }
};
```



### 困难

#### [1250. 检查「好数组」](https://leetcode-cn.com/problems/check-if-it-is-a-good-array/)

> 给你一个正整数数组 `nums`，你需要从中任选一些子集，然后将子集中每一个数乘以一个 **任意整数**，并求出他们的和。
>
> 假如该和结果为 `1`，那么原数组就是一个「**好数组**」，则返回 `True`；否则请返回 `False`。
>
> **示例 1：**
>
> ```
> 输入：nums = [12,5,7,23]
> 输出：true
> 解释：挑选数字 5 和 7。
> 5*3 + 7*(-2) = 1
> 
> 
> ```

##### 思路

规律是如果这些数的最大公约数是1，那么就是好数组。规律以后再谈

- 时间复杂度On，空间O1

##### 代码


```c++
class Solution {
public:
    bool isGoodArray(vector<int>& nums) {
        int g=nums[0];
        for(int i=1;i<nums.size();i++){
            g=gcd(g,nums[i]);
        }
        return g==1;
    }
    int gcd(int a,int b){
        int c=a%b;
        while(c!=0){
            a=b;
            b=c;
            c=a%b;
        }
        return b;
    }
};
```
