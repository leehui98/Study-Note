# 0.基础知识

## string 

```cpp
string s;

s.erase(0,1);//删除第0个元素及其后面的共1个元素

s.insert(s.begin(),'1');//在第0个元素插入字符'1'

s.substr(int pos,int num);//pos是位置，num是从这个位置开始的数量

reverse(s.begin(),s.end());//反转字符串
```

## unordered_map

```cpp
unordered_map<int,int> memo;

memo.erase(key);
```

## priority_queue

```cpp
#include<priority_queu>

priority_queue<int,vector<int>,less<int>> q; //大顶堆，根节点比子树大

priority_queue<int,vector<int>,greater<int>> q;//小顶堆，根节点比子树小
empty、size、front、push_back、pop_back
```

```cpp
less<int> l;
l(int a,int b){return a<b;}//满足小于符号返回true
```

## vector

```cpp
vector<int> vec;
vec.erase(vec.begin());
vec.insert(vec.begin()+1,1);
```



# 1.双指针

# 2.动态规划

# 3. 回溯算法

求子集、求排列、求组合

穷举则用回溯。

回溯三要素：记录路径，选择列表，结束条件。

框架：结束条件--选择列表做选择，去递归，撤销选择

## 1.全排列

### 1.一个字符串/整数的全排列

程序运行的结果是比所给整数大的最小的数字，使用的方法是全排列

```CPP
#include<iostream>
#include<unordered_map>
#include<vector>
#include<sstream>
#include<algorithm>
using namespace std;
int res=INT_MAX;
int num;
void getRes(string s,string trace){
    if(s.size()==0){
        int ans=0;
        for(int i=0;i<trace.size();i++){
            ans=ans*10+trace[i]-'0';
        }
        if(ans>num&&ans<res) res=ans;
        return ;
    }
    for(int i=0;i<s.size();i++){
        char t=s[i];
        s.erase(i,1);
        getRes(s,trace+t);
        s.insert(s.begin()+i,t);
    }
}
int main()
{
    stringstream st;
    string s="112131321";
    st<<s;st>>num;
    getRes(s,"");
    cout<<res;
}

```

### 2.正确答案：从后往前找，找到后面的某个数字比前面大的相差最小的那一个，并将后面的数字从小到大排列

```cpp
int res2;
auto cmp=[](const pair<char,int> &a,const pair<char,int> &b){return a.first<b.first;};
string getRes2(string& s){
    string res=s;
    int index;
    priority_queue<pair<char,int>,vector<pair<char,int>>,decltype(cmp)> q(cmp);
    for(int i=res.size();i>=0;i--){
        if(!q.empty()&&q.top().first>res[i]){
            index=i;
            int temp;
            while(!q.empty()&&q.top().first>s[i]){
                temp=q.top().second;
                q.pop();
            }
            char t=res[temp];
            res[temp]=res[index];
            res[index]=t;
            break;
        }
        q.push({s[i],i});
    }
    sort(res.begin()+index+1,res.end());
    return res;
}
int main()
{
    string s="112131321";
    cout<<getRes2(s);
}
```

### 3.返回一个数组的全排列

```cpp
#include<iostream>
#include<vector>
using namespace std;
vector<vector<int>> res;
void getRes(vector<int> &nums,vector<int>& ans)
{
    if(nums.size()==0)
    {
        res.push_back(ans);
    }
    for(int i=0; i<nums.size(); i++)
    {
        int t=nums[i];
        nums.erase(nums.begin()+i);
        ans.push_back(t);
        getRes(nums,ans);
        ans.pop_back();
        nums.insert(nums.begin()+i,t);
    }
}
int main()
{
    vector<int> ans;
    vector<int> nums= {1,2,3};
    getRes(nums,ans);
    for(int i=0; i<res.size(); i++)
    {
        for(int j=0; j<res[i].size(); j++)
        {
            cout<<res[i][j];
        }
        cout<<endl;
    }
}

```



## 2.子集

一个数组的全部子集。leetcode-76

描述：返回数组中所有的子集

输入：nums = [1,2,3]，输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

思路1：回溯，选择不选择

```cpp
#include<iostream>
#include<vector>
using namespace std;
vector<vector<int>> res;
void getRes(int index,vector<int> &nums,vector<int> ans){
    if(index==nums.size()){
        res.push_back(ans);
        return ;
    }
    getRes(index+1,nums,ans);
    ans.push_back(nums[index]);
    getRes(index+1,nums,ans);
}
int main(){
    res.clear();
    vector<int> nums{1,2,3};
    getRes(0,nums,{});
    for(int i=0;i<res.size();i++){
        for(int j=0;j<res[i].size();j++){
            cout<<res[i][j]<<" ";
        }
        cout<<endl;
    }
}
```

思路2：递归得到子集。返回的时候要求几个vector就要返回几个中括号。

## 3.组合

leetcode-77

## 4.数独

```cpp
#include<iostream>
#include<vector>
using namespace std;
bool isValid(vector<vector<char>> board,int r,int c,char ch){
    for(int i=0;i<9;i++)
        {

            if(board[i][c]==ch) return false;
            if(board[r][i]==ch) return false;
            if(board[(r/3)*3+i/3][(c/3)*3+i%3]==ch) return false;
    }
    return true;

}
bool getRes(vector<vector<char>> &board,int r,int c){
    int m=9;
    int n=9;
    if(c==n){
        return getRes(board,r+1,0);
    }
    if(r==m){
        return true;
    }
    for(int i=r;i<m;i++)
    for(int j=c;j<n;j++){
        if(board[i][j]!='.')
            return getRes(board,i,j+1);
        for(int ch='1';ch<='9';ch++){
            if(!isValid(board,i,j,ch)){
                continue;
            }
            board[i][j]=ch;
            if(getRes(board,i,j+1)){
                return true;
            }
            board[i][j]='.' ;
        }
        return false;
    }
    return false;
}


int main(int argv,char* argc[]){
    vector<vector<char>> board{{'5','3','.','.','7','.','.','.','.'},{'6','.','.','1','9','5','.',\
    '.','.'},{'.','9','8','.','.','.','.','6','.'},{'8','.','.','.','6','.','.','.','3'},{'4','.',\
    '.','8','.','3','.','.','1'},{'7','.','.','.','2','.','.','.','6'},{'.','6','.','.','.','.','2',\
    '8','.'},{'.','.','.','4','1','9','.','.','5'},{'.','.','.','.','8','.','.','7','9'}};
    getRes(board,0,0);
    for(int i=0;i<board.size();i++)
    {
        for(int j=0;j<board[i].size();j++){
                cout<<board[i][j];
        }
        cout<<endl;
    }
    return 1;


}

```



## leetcode-22.括号生成

描述：给一个数字，返回所有n对括号的有效组合。

输入：n=3，输出：["((()))","(()())","(())()","()(())","()()()"]

方法1：穷举所有组合

方法2：left记录剩余朝向左的括号量，right记录剩余朝向右的括号量。因为右'('剩余的比左')'少，如果right<left就不合法，返回；如果left或right小于0，也不合法，返回；如果left==0&&right==0，合法，添加到res中。

```cpp
class Solution {
public:
    void getRes(int left,int right,string ans,vector<string>& res){
        if(left<right) return ;
        if(left<0||right<0) return ;
        if(left==0&&right==0) {
            res.push_back(ans);
            return ;
        }
        getRes(left-1,right,ans+')',res);
        getRes(left,right-1,ans+'(',res);

    }
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        getRes(n,n,"",res);
        return res;
    }
};
```







# 4.树

# 5.BFS

计算最小步数、最短距离

从一个点开始，向周围扩散。使用队列实现，每次将一个节点附近的节点加入队列。

出现场景：在一幅图中，找到起点start到target的最近距离

框架：

```cpp
int bfs(Node start,Node target){
    queue<Node> q;
    unordered_map<Node> memo;
    
    q.push(start);
    memo[start]=1;
    
    int step=0;
    
    while(!q.empty()){
        int len=q.size();
        for(int i=0;i<len;i++){
            Node cur;
            if(cur==target)
                return step;
            for(Node x:cur.adj()){
                q.push(x);
                memo[x]=1;
            }
        }
        step++;
    }
}//bf就是使用队列实现剥洋葱
//核心：cur.adj();泛指cur相邻的节点，比如说二维数组中，cur上下左右四个点就是相邻节点，memo主要是防止走回头路
```

## leetcode-111 二叉树的最小深度

返回根节点到最近叶子节点的距离

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root==NULL) return 0;
        int step=1;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int size=q.size();
            for(int i=0;i<size;i++){
                TreeNode* cur=q.front();
                if(!cur->left&&!cur->right) return step;
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
                q.pop();
            }
            step++;
        }
        return step;
    }
};
```



BFS是集体行动,DFS是单独行动

## leetcode-752 打开转盘锁

```cpp
//要注意在push到队列的时候,添加备忘录
class Solution
{
public:
    vector<int> operation{-1,1};
    int openLock(vector<string>& deadends, string target)
    {
        unordered_map<string,int> memo;
        for(string s:deadends)
        {
            memo[s]=1;
        }
        if(memo.find("0000")!=memo.end()) return -1;
        queue<string> q;
        int step=0;
        q.push("0000");
        memo["0000"]=1;
        while(!q.empty())
        {
            int size=q.size();
            for(int i=0; i<size; i++)
            {
                string cur=q.front();
                q.pop();
                if(cur==target) return step;
                for(int j=0; j<4; j++)
                {
                    for(int k=0; k<operation.size(); k++)
                    {
                        string s=cur;
                        if(s[j]=='0'&&k==0) s[j]='9';
                        else if(s[j]=='9'&&k==1) s[j]='0';
                        else s[j]+=operation[k];
                        if(memo.find(s)==memo.end())
                        {
                            q.push(s);
                            memo[s]=1;
                        }
                    }
                }
            }
            step++;
        }
        return -1;
    }
};

```



# 6.DFS

# 7.哈希表

## leetcode-380. 常数时间插入、删除和获取随即元素

## leetcode-710. 黑名单中的随机数

## leetcode-1. 两数之和

给一个数组，和一个目标值target，返回能组合成target的下标

思路：哈希表，一遍循环，满足条件则返回，不满足条件放入哈希表中

```cpp
#include<iostream>
#include<unordered_map>
#include<vector>
using namespace std;
int main()
{
    vector<int> nums= {2,3,4,5,6};
    int target=10;
    unordered_map<int,int> memo;
    for(int i=0; i<nums.size(); i++)
    {
        if(memo.find(target-nums[i])==memo.end())
        {
            memo[nums[i]]=i;
        }
        else
        {
            cout<<i<<" "<<memo[target-nums[i]];
            return 1;
        }
    }
    return 1;
}
```

两数之和进阶思考

如果每次调用find，要考虑使用哈希map在添加的时候将和进行存储，find的时候时间复杂度就是O(1)。

# 8. 双指针/滑动窗口

## 8.1 二分查找

- 查找一个数[left,right]，left=mid+1,right=mid-1,return mid;
- 查找左边界[left,right)，left=mid+1,right=mid,return left;
- 查找有边界[left,right)，left=mid+1,right=mid.return left-1;



# 其他算法



## 1.前缀和

leetcode-560. 和为K的子数组

适用于原始数组不会修改的情况，频繁查询某个区间的累加和。想求[i,j]的区间，用prefix[J+1]-pre[i]。prefix[i]表示[0,i)之间的元素和。

场景：和为k的子数组的个数

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> memo;
        vector<int> preSum(nums.size()+1,0);
        memo[0]=1;
        int res=0;
        for(int i=0;i<nums.size();i++){
            preSum[i+1]=nums[i]+preSum[i];
            res+=memo[preSum[i+1]-k];
            memo[preSum[i+1]]++;
        }
        return res;
    }
};
```

## 2.差分数组

使用场景是频繁对原始数组的某个区间的元素进行增减。核心：<font color=red>改变区间元素</font>

res[0]=diff[0]

res[i]=res[i-1]+diff[i].

leetcode-1109:航班飞机

依次对航班飞机预定（依次修改数组某个区间中的元素），最后返回预定结果（最后返回数组被修改后的结果）

```cpp
#include<iostream>
#include<vector>
using namespace std;
#define show(a) cout<<a;
int main(){
    vector<vector<int>> bookings = {{1,2,10},{2,3,20},{2,5,25}};
    int n = 5;
    vector<int> diff(n,0);
    vector<int> res(n,0);
    for(int i=0;i<bookings.size();i++){
        int index1=bookings[i][0]-1;
        int index2=bookings[i][1]-1;
        int value=bookings[i][2];
        diff[index1]+=value;
        if(index2+1<n) diff[index2+1]-=value;
    }
    res[0]=diff[0];
    for(int i=1;i<n;i++){
        res[i]=res[i-1]+diff[i];
    }
    for(int i=0;i<res.size();i++){
        cout<<res[i]<<" ";
    }
}
```

## 3. 快速选择算法

给一个无序数组nums，和一个正整数k，返回nums中第k大的数

```cpp
//快速排序
void qsort(vector<int> &nums,int left,int right){
    if(left<right){
        int i=left,j=right;
        int x=nums[left];
        while(i<j){
            while(nums[j]>=x&&i<j){
                j--;
            }
            if(i<j) nums[i]=nums[j];
            while(nums[i]<=x&&i<j){
                i++;
            }
            if(i<j) nums[j]=nums[i];
        }
        nums[i]=x;
        qsort(nums,left,i-1);
        qsort(nums,i+1,right);
    }
}
//保持边界条件，最后把坑填上。left开始，j--。下一次递归式(left,i-1)；(i+1,right)。其实left<=right和left<right都可以

```

```cpp
class Solution {
public:
    int getK(vector<int> &nums,int left,int right,int k){
        if(left<=right){
            int i=left,j=right,x=nums[left];
            while(i<j){
                while(nums[j]<=x&&i<j) j--;
                if(i<j) nums[i++]=nums[j];
                while(nums[i]>=x&&i<j) i++;
                if(i<j) nums[j--]=nums[i];
            }
            nums[i]=x;
            if(i==k-1) return nums[i];
            else if(i<k-1) return getK(nums,i+1,right,k);
            else if(i>k-1) return getK(nums,left,i-1,k);
        }
        return -1;
    }
    int findKthLargest(vector<int>& nums, int k) {
        return getK(nums,0,nums.size()-1,k);   
    }
};
//类似于快排，如果get到i==k-1,直接返回，否则去左边或者右边。注意结束条件是left<=right
```



## 4.分治算法：表达不同的优先级

leetcode-241. 为运算表达式设计优先级

核心思路：遇到+-*，分而治之，相当于二叉树的后序遍历

```cpp
class Solution
{
public:
    vector<int> diffWaysToCompute(string expression)
    {
        vector<int> res;
        for(int i=0; i<expression.size(); i++)
        {
            char ch=expression[i];
            if(ch=='+'||ch=='-'||ch=='*')
            {
                vector<int> left=diffWaysToCompute(expression.substr(0,i));
                vector<int> right=diffWaysToCompute(expression.substr(i+1,expression.size()-i-1));

                for(int j=0; j<left.size(); j++)
                    for(int k=0; k<right.size(); k++)
                    {
                        switch(ch)
                        {
                        case '+':
                            res.push_back(left[j]+right[k]);
                            break;
                        case '-':
                            res.push_back(left[j]-right[k]);
                            break;
                        case '*':
                            res.push_back(left[j]*right[k]);
                            break;
                        default:
                            break;
                        }
                    }
            }
        }
        if(res.size()==0)
        {
            int num;
            stringstream st;
            st<<expression;
            st>>num;
            res.push_back(num);
        }
        return res;
    }
};

```

