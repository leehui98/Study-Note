# 1.双指针

# 2.动态规划

# 3. 回溯算法

穷举则用回溯

## leetcode-22.括号生成

描述：给一个数字，返回所有n对括号的有效组合。

输入：n=3，输出：["((()))","(()())","(())()","()(())","()()()"]

思路：穷举所有组合，判断组合是否有效。

穷举：一共有2n个位置，每个位置选择"("或")"，字符串到达2n后测试是否有效。

代码：

```cpp
#include<iostream>
#include<vector>
using namespace std;
vector<string> res;

bool balance(string s){
    int flag=0;
    for(int i=0;i<s.size();i++){
        if(s[i]=='(') flag++;
        else flag--;
        if(flag<0) return false;
    }
    if(flag!=0) return false;
    return true;
}
void getRes(int index,int total,string ans){
    if(index==total) {
        if(balance(ans)) res.push_back(ans);
        return ;
    }
    getRes(index+1,total,ans+"(");//做选择
    //撤销
    getRes(index+1,total,ans+")");//做选择
    //撤销
}

int main(){
    res.clear();
    int n=3;
    getRes(0,2*n,"");
    for(int i=0;i<res.size();i++)
        cout<<res[i]<<endl;;
}

```

## leetcode-78.子集

描述：返回数组中所有的子集

输入：nums = [1,2,3]，输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

思路：回溯，选择不选择

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







# 4.树

# 5.BFS

# 6.DFS

# 7.哈希表

## leetcode-380 常数时间插入、删除和获取随即元素

