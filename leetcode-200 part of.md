### 156 二叉树翻转. 

没理解感觉

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* upside(TreeNode* root)
    {
        if(root == NULL)
        {
            return NULL;
        }
        if(root->left == NULL  && root->right == NULL)
        {
            return root;
        }
        TreeNode* tmp_root = root->left;
        TreeNode* leftnode = upside(root->left);
        tmp_root->left = root->right;
        tmp_root->right = root;
        root->left = NULL;
        root->right = NULL;
        return leftnode;

    }
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
        if(root == NULL)
        {
            return NULL;
        }
        if(root->left == NULL  && root->right == NULL)
        {
            return root;
        }
        TreeNode* tmp_root = root->left;
        TreeNode* leftnode = upsideDownBinaryTree(root->left);
        tmp_root->left = root->right;
        tmp_root->right = root;
        root->left = NULL;
        root->right = NULL;
        
        return leftnode;
    
    }
};
```



### leetcode 157

```C++
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char *buf4);
 */

class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
  int temp = 0, ret = 0;
  char *tmpbuf = new char[4];
  
    int read(char *buf, int n) {
        int temp = 0, ret = 0;
        char *tmpbuf = new char[4];

        while(temp <= n)
        {
          ret = read4(tmpbuf);
          if(!ret)
          {
              break;
          }
          for(int i=0; i<ret && temp<n; i++)
          {
              buf[temp++] = tmpbuf[i];
          }
        }
        return temp;
    }
};

// 范例代码
int read(char *buf, int n) {
        int ans=0;
        int cnt;
        for(int i=0;i<n;i+=cnt){
            //从buf+i开始尝试读取4个字符到buf中，返回实际读取个数
            cnt=read4(buf+i);
            if(cnt==0)return ans;
            ans+=cnt;
        }
        //返回实际读取的个数
        return min(ans,n);
    }
```

### leetcode 158 

多次调用read4

```C++
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char *buf4);
 */

class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int buffer_left =0;
    int buffer_size =0;
    char *tmpbuf = new char[4];
    // bool isfirst = true;
    // bool endoffile = false;
    int read(char *buf, int n) {
        int temp = 0,ret = 0;
        
        while(temp < n)
        {
          if(buffer_left <= 0)
          {
              ret = read4(tmpbuf);
              if(!ret)
               {
                    return temp;
               }
              buffer_left = ret;  
              buffer_size = ret;        
          }         
          for(int i=0; i<buffer_left && temp<n; i++)
          {
              buf[temp++] = tmpbuf[buffer_size-buffer_left];
              buffer_left-- ;
              if(buffer_left <=0 && temp<n)
              {
                ret = read4(tmpbuf);
                if(!ret)
                {
                    return temp;
                }
                buffer_left = ret; 
                buffer_size = ret;
              }
          }
        }
        return temp;
    }
};
// 快速版本的 
class Solution {
public:
    int len = 0;
    char buf4[4];
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int tmp = 0;
        strncpy(buf, buf4, len);
        tmp += len;
        //for (int i = 0; i < len; i++) {
        //    buf[tmp++] = buf4[i];
        //}
        for (int i = tmp; i < n; i += 4) {
            tmp += read4(buf + i);
        }
        len = 0;
        for (int i = n; i < tmp; i++) {
            buf4[len++] = buf[i];
        }
        return n > tmp ? tmp : n;
    }
};
```



### leetcode159

用dp节2可能有点麻烦？ 官方用的双指针

```C++
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        int ans = 0; 
        if(s.length() <= 2)
        {
            return s.length();
        }
        char s1 = s[0];
        char s2 = s[0];
        int left = 0;
        int right = 0;
        bool flag = false;
        vector<int> dp(s.length(),0);
        dp[0] = 0;
        dp[1] = 2;
        for(int i=2;i<s.length();i++)
        {
            if(s[i] == s[i-1])
            {
                dp[i] = dp[i-1]+1;
            }
            else 
            {
                flag = false;
                dp[i] = 2;
                int j=1;
                for(j=1; j<=dp[i-1];j++)
                {
                    if(s[i-j] != s[i] && s[i-j]!=s[i-1])
                    {
                        break;
                    }
                }    
                dp[i] = j;
            }
        }
    for(int i=0;i<s.length();i++)
    {
        ans = max(ans, dp[i]);
    }
    return ans;
    }
};
```



### leetcode 161 相隔为1

要考虑多种情况， 完全相同的   "",""的 ， "a","" 的 ,aca caca

```C++
bool isOneDifL(string s, string t)
    {
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == t[i])
                continue;
            return s.substr(i) == t.substr(i+1);
        }
        return true; // 说明前面的都相等只有最后一个不匹配
    }
    bool isSameLen(string s, string t)
    {
        int cnt = 0;
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] != t[i])
                cnt++;
        }
        return cnt == 1;
    }
    bool isOneEditDistance(string s, string t)
    {
        if (abs(s.size() - t.size()) > 1) return false;
        if (s.size() == t.size()) return isSameLen(s, t);
        else if (s.size() < t.size()) return isOneDifL(s, t);
        else return isOneDifL(t, s);
    }

// SB 的实现
class Solution {
public:
    bool isOneEditDistance(string s, string t) {
        int lens = s.length();
        int lent = t.length();
        if(abs(lens - lent) >=2 || ((!lens) &&(!lent)) )
        {
            return false;
        }
        int lenmin = min(lens, lent);
        int cntdiff = 0;
        int diffpos = 0;
        bool haschange = false;
        if(lens != lent)
        {
            haschange = true;
        }
        if(lens < lent)
        {
            swap(s,t) ;   // s is always the long 
        }
        for(int i=0 , j=0; j < lenmin;i++, j++)
        {
                if(s[i] != t[j])
                {
                    haschange = true;
                    diffpos = j;
                    cntdiff++;
                 if(cntdiff>=2)
                  {
                    return false;
                  }
                   if(lens != lent)
                    {
                        for(int k = diffpos ; k<lenmin; k++)
                        {
                            if(s[k+1] != t[k])
                            {
                                return false;
                            }
                        }
                        return true;
                    }
                }               
                
        }
        if(!haschange)
        {
            return false;
        }
        return true;
    }
};
```

### leetcode 255.

回溯+单调栈 用BST性质， 

```C++
class Solution {
public:
   bool dfs(vector<int>& preorder, int start, int end, int maxval, int minval )
{
    if(start > end) return true;
    int root = preorder[start];
    if(root <= minval || root >= maxval)
    {
        return false;
    }
//  find the right child
    int i = start +1;
    for(i = start +1; i <= end ; i++)
    {
        if(preorder[i] > root)
        {
            break;
        }
    }
    int rchild  = i;
    return dfs(preorder, start+1, rchild -1, root, minval) && dfs(preorder, rchild, end, maxval, root);
}

    bool verifyPreorder(vector<int>& preorder) {
    // int maxval = 1e9+1;
    // int minval = -1;
    // return dfs(preorder,0, preorder.size()-1, maxval, minval);
    // another way mono stack
    
    stack<int> st;
    int cnt = 0, temp, lasttop = -10000;
    st.push(preorder[0]);
    if(preorder.size() == 1)
    {
        return true;
    }
    while(!st.empty())
    {

        temp = preorder[++cnt];
        while(!st.empty() && st.top() < temp)
        {
            lasttop = st.top();
            st.pop();
        }
        if(lasttop > temp)
        {
            return false;
        }
        st.push(temp);
        if(cnt >= preorder.size() -1) {
            break;
        }
    }
    return true;
}   
};
```



### leetcode 163 缺失区间

 注意各种测试用例 [-1] (-2,-1)  区间为空 [-1] -1 0 

```C++
class Solution {
public:
    string num2str(int num)
    {
        int n = num;
        string str= "";
        bool neg = false;
        if(num < 0)
        {
            neg = true;
            n*=-1;
        }
        if(n == 0)
        {
            str += "0";
        }
        while(n)
        {
           str +=   n%10 + '0';
           n/=10;
        }
        if(neg)
        {
            str += "-";
        }
         reverse(str.begin(), str.end());
         return str;
    }
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<string> res ;
        if(lower > upper)
        {
            return res;
        }
        if(nums.size() == 0)
        {
            if(upper - lower > 1)
            {
                string s = num2str(lower) +"->" +num2str(upper);
                res.push_back(s);
                return  res;
            }
            else 
            {
                string s = num2str(lower) ;
                res.push_back(s);
                return  res;
            }
        }
        if(nums[0] - lower >= 2)
        {
            string s = num2str(lower)+"->" +num2str(nums[0] - 1) ;
            res.push_back(s);
        }
        if(nums[0] - lower ==1)
        {
            string s = num2str(lower);
            res.push_back(s);
        }
        for(int i=0; i<nums.size() ; i++)
        {
            if(nums[i] < lower)
            {
                continue;
            }
            if(nums[i] > upper)
            {
                break;
            }
            if(i+1 < nums.size() && min(nums[i+1]-1, upper) - nums[i] > 1)
            {
                string s = num2str(nums[i]+1) +"->" +num2str(min(nums[i+1]-1, upper));
                res.push_back(s);
            }
            if(i+1 < nums.size() && min(nums[i+1]-1, upper) - nums[i]  ==1 )
            {
                string s = num2str(nums[i] + 1);
                res.push_back(s);
            }
            if(i+1 == nums.size() &&  upper - nums[i] >=2 )
            {
                string s = num2str(nums[i]+1) +"->" + num2str(upper);
                res.push_back(s);
            }
            if(i+1 == nums.size() &&  upper - nums[i] == 1)
            {
                string s =  num2str(upper);
                res.push_back(s);
            }
            
        }
    return res;
    }
};
```







### leetcode 170 两数之和

考虑的问题 排序，分析函数调用次数

```C++
class TwoSum {
    unordered_map<long, int> mp;
public:
    TwoSum() {
        mp.clear();
    }
    
    void add(int number) {
        mp[number]++;
    }
    
    bool find(int value) {
        for(auto it = mp.begin(); it != mp.end(); it++) {
            auto found = mp.find(value - it->first);
            if (found != mp.end()) {
                if (found->first != it->first || (found->first == it->first && mp[found->first] > 1)) {
                    return true;
                }
            }
        }
        return false;
    }
};

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum* obj = new TwoSum();
 * obj->add(number);
 * bool param_2 = obj->find(value);
 */
```



### leetcode 256 粉刷房子 dp

```C++
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        int n = costs.size();
        // only 3 colors
        int pre[3] = {0}, cur[3] = {0};
        for (int i = 0; i < n; ++i){
            for (int j = 0; j < 3; ++j){
                cur[j] = costs[i][j] + min(pre[(j + 1) % 3], pre[(j + 2) % 3]);
            }
            swap(pre, cur);
        }
        
        return min(pre[0], min(pre[1], pre[2]));
    }
};
```



### leetcode 257 粉刷房子改进dp 

```
 int &d = dp[i + 1][j];//d来表示dp[i + 1][j], 代码看起来干净

                //如果当前颜色j与上一个房子的花费最小颜色花费不一样，就可以直接去上一个房子颜色的最低花费，与当前花费形成最优解，如果一样，就取上一个房子的第二低花费来形成当前颜色的最优解
                if(dp[i][j] != c1) d = costs[i][j] + c1;
                else d = costs[i][j] + c2;

                //在得到当前房子的最优解时候同时为下一个房子求出最低花费和第二低花费
                //就是维护一个最小值，和一个第二小的值
                if(d < tmp1) tmp2 = tmp1, tmp1 = d;
                else if(d < tmp2) tmp2 = d;
            }
            c1 = tmp1, c2 = tmp2;//换上新的值

作者：cyzh
链接：https://leetcode-cn.com/problems/paint-house-ii/solution/dong-tai-gui-hua-nkfu-za-du-jian-dan-yi-dof32/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### leetcode 

理解错题了

```C++
    void reversesub(int st, int en, vector<char>& s)
    {
        if(en - st <= 1)
        {
            return;
        }
        int mid = (en + st)/2;
        for(int i=0; st+i <=mid ; i++)
        {
            swap(s[st+i], s[en-i]);
        }
        return ;
    }
    void reverseWords(vector<char>& s) {
        int low = 0, high = 0;
        if(s.size() <=1)
        {
            return ;
        }
        int i=0;
        for( i =0;i<s.size() ; i++)
        {
            while(s[high] != ' ' && high < s.size())
            {
                high ++;
                if(high == s.size())
                {
                    break;
                }
            }
            reversesub(low, high-1,s);
            low = ++high;
            if(low >= s.size())
            {
                return;
            }
        }
        return ;
    }
};

class Solution {
public:
    void reversewd(int st, int en, vector<char>& s)
    {
        if(en - st < 1 )
        {
            return ;
        }
        int mid = (st + en)/2;
        for(int i=0; st+i<=mid; i++)
        {
            swap(s[st + i] , s[en - i]);
        }
        return ;
    }
    void reverseWords(vector<char>& s) {
        int low = 0, high = 0;
        if(s.size() <=1)
        {
            return ;
        }
        int i=0;
        int st, en;
        en = s.size();
        for( i =s.size()-1;i >=0 ; i--)
        {
            if(s[i] == ' ')
            {
                reversewd(i+1,en-1,s); 
                en = i;  // the end of next word
            }            
            if(i == 0)
            {
                reversewd(i, en-1, s);
            }
        }
        reversewd(0,s.size()-1, s);
        return ;
    }
};
```



### leetcode 99

注意是中序遍历+ 栈 快写！

MORRIS leetcode 234

### leetcode 234/244 

```C++
class Solution {
public:
    int shortestDistance(vector<string>& wordsDict, string word1, string word2) {
        int mindis = wordsDict.size();
        int wd1pos = -1;
        int wd2pos = -1;
        for(int i=0; i<wordsDict.size(); i++)
        {
            if(wordsDict[i] == word1)
            {
                if(wd2pos != -1)
                {
                    mindis = min(mindis, abs(wd2pos - i));
                }
                wd1pos = i;
            }
            if(wordsDict[i] == word2)
            {
                if(wd1pos != -1)
                {
                    mindis = min(mindis, abs(wd1pos - i));
                }
                wd2pos = i;
            }
        }
        return mindis;
    }
    
};

// 244
class WordDistance {
public:
    unordered_map<string, vector<int>>dicts;
    WordDistance(vector<string>& wordsDict) {
        for(int i=0; i<wordsDict.size() ;i++)
        {
            dicts[wordsDict[i]].push_back(i);
        }
    }
    
    int shortest(string word1, string word2) {
        vector<int> vec1 = dicts[word1];
        vector<int> vec2 = dicts[word2];
        int wd1pos = 0;
        int wd2pos = 0;
        int mindis = 1e9+1;  // 这里面能找到wordsdict
        while(wd1pos < vec1.size() && wd2pos < vec2.size())
        {
             mindis = min(mindis, abs(vec1[wd1pos] - vec2[wd2pos]));
             if(vec1[wd1pos] < vec2[wd2pos])
             {
                 wd1pos+=1;
             }
             else if(vec1[wd1pos] > vec2[wd2pos])
             {
                 wd2pos += 1;
             }
        }
        return mindis;
    }
};

/**
 * Your WordDistance object will be instantiated and called as such:
 * WordDistance* obj = new WordDistance(wordsDict);
 * int param_1 = obj->shortest(word1,word2);
 */
// 245
class Solution {
public:
    int shortestWordDistance(vector<string>& wordsDict, string word1, string word2) {
        int idx1 = -1, idx2 = -1;
        int ans = INT_MAX;

        for(int i = 0; i < wordsDict.size(); i++) {
            if(wordsDict[i] == word1) {
                idx1 = i;
                if (idx2 >= 0) {
                    ans = min(ans, idx1 - idx2);
                }
            }
            if (wordsDict[i] == word2) {
                idx2 = i;
                if (idx1 >= 0 && idx1 != idx2) {
                    ans = min(ans, idx2 - idx1);
                }
            }
        }
        return ans;
    }
};
```



### leetcode 250 统计同值子树

同值指的是 子树上每个节点值相同

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int count = 0;
    bool check(TreeNode* root)
    {
        if(!root) return true;
        bool lchild = check(root->left);
        bool rchild = check(root->right);
        if( !lchild ||  !rchild || (root->left && root->val != root->left->val) || (root->right && root->val != root->right->val) )
        {
            return false;
        }
        count++;
        return true;
    }
    // bool checksame(TreeNode* root1, TreeNode* root2)
    // {
    //     if(!root1 && !root2)
    //     {
    //         return true;
    //     }
    //     if( (!root1 && root2) || (root1 && !root2) || (root1->val != root2->val) )
    //     {
    //         return false;
    //     }
    //     return checksame(root1->left, root2->left) && checksame(root1->right, root2->right);
    // }


    int countUnivalSubtrees(TreeNode* root) {
        check(root);
        return count;
    }
};
```



### leetcode 250

```C++
// 注意测试用例
["Vector2D","hasNext","next","hasNext"]
[[[[1],[]]],[],[],[]]

// 这里[]是一个向量
  
class Vector2D 
{
public:
    vector<int> nums;
    int idx;
    Vector2D(vector<vector<int>>& v) 
    {
        for (auto sub_v: v)
        {
            for (int x: sub_v)
            {
                nums.push_back(x);
            }
        }
        idx = 0;
    }
    
    int next() 
    {

        int res = nums[idx];
        idx ++;
        return res;
    }
    
    bool hasNext() 
    {
        return idx < nums.size();
    }
};

// 我的傻逼做法  有问题
class Vector2D {
public:
     int strow = 0;  // 第几个向量
     int stcol = 0;  // 该向量的第几个元素
    vector<int> lens;
    vector<vector<int>>vec1;
    Vector2D(vector<vector<int>>& vec) {
        for(int i=0; i<vec.size() ; i++)
        {
            lens.push_back(vec[i].size());
        }
        vec1 = vec;
    }
    
    int next() {
        if(stcol < lens[strow])
        {
            stcol+=1;
            return vec1[strow][stcol-1];  // vec not define
        }
        else if(stcol == lens[strow])
        {
            
            if(strow+1 < vec1.size())
            {
                strow += 1;
                stcol = 1;
                return vec1[strow][stcol-1];
            }
           // stcol +=1;  // 辅助判断有没有到达最后一个元素
           
        }
         return -1;
    }  
    bool hasNext() {
        cout<< vec1.size()<<endl;
        if(vec1.size() == 0 || strow > vec1.size())
        {
            return false;
        }
        if(strow >= vec1.size() -1 && stcol >= lens[vec1.size()-1] )
        {
            return false;
        }
        if(vec1[strow].size() == 0)
        {
            return false;
        }
        return true;
    }
};
```



### leetcode 252

```C++
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        int n = intervals.size();
        for(int i=0; i<n-1; i++){
            if(intervals[i][1]>intervals[i+1][0]){
                return false;
            }
        }
        return true;
    }
};
// 我的傻逼写法

```



### leetcode 253

查分方法非常慢

```C++
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int maxroom = -1;
        int maxpos = 0;
        vector<int> diff(10e6 + 1, 0);
        for(auto x : intervals)
        {
            diff[x[0]]++;   //  
            diff[x[1]]--;
            maxpos = max(maxpos, x[1]);
        }
        for(int i=1; i<= maxpos;i++)
        {
            diff[i] +=diff[i-1];
            maxroom  = max(maxroom , diff[i]);
        }
        return maxroom;
    }
};
// 我的SB方法 最小堆，也可以直接优先队列
class Solution {
public:
    static bool cmp(vector<int> a , vector<int> b)
    {
        if(a[0] == b[0])
        {
            return a[1] < b[1];
        }
        return a[0]<b[0];
    }
    void sift_up(vector<vector<int>>& heap)
    {
        int end = heap.size() -1;
        vector<int> val = heap[end];
        while(end/2>0 && val[1] < heap[end/2][1])
        {
            heap[end] = heap[end/2];
            end/=2;
            heap[end] = val;
        }
    }
    void heap_insert(vector<vector<int>>& heap, vector<int> k)
    {
        heap.push_back(k);
        sift_up(heap);
    }
    void delete_min(vector<vector<int>>& heap)
    {
    if(heap.size() == 0)
    {
        return ;
    }
    int par = 0;
    int child = 1;
    int end = max(int(heap.size()-1),0);
    
    vector<int> min_vec = heap[0];
    vector<int> max_vec = heap[end];
    for(par=0; par*2 < end ; par = child)
    {
        child = par*2+1;
        if(par != end  && heap[child][1] > heap[child+1][1] )
        {
         child+=1;
        }
        if(max_vec[1] > heap[child][1])
        {
            heap[par] = heap[child];
        }
        else
        {
        break;
        }
    }
    heap[par]  =max_vec;  // 还是heap[child]
    
    
    return ;
    }



    int minMeetingRooms(vector<vector<int>>& intervals) {
        if(intervals.size() == 0)
        {
            return 0;
        }
        sort(intervals.begin(), intervals.end(), cmp);
        vector<vector<int>> heap;
        heap_insert(heap, intervals[0]);
        for(int i=1;i<intervals.size(); i++)
        {
            if(heap[0][1] >  intervals[i][0]) // 最早结束的会议 结束时间比现在还要晚
            {
                heap_insert(heap, intervals[i]);
            }
            else{
                heap[0] = intervals[i];
                sift_up(heap);
            }
        }
        return heap.size();

    }
};
//     // 区间重合 TLE
//     int minMeetingRooms(vector<vector<int>>& intervals) {
//         int maxroom = -1;
//         int maxpos = 0;
//         vector<int> diff(10e6 + 1, 0);
//         for(auto x : intervals)
//         {
//             diff[x[0]]++;   // 
//             diff[x[1]]--;
//             maxpos = max(maxpos, x[1]);
//         }
//         for(int i=1; i<= maxpos;i++)
//         {
//             diff[i] +=diff[i-1];
//             maxroom  = max(maxroom , diff[i]);
//         }
//         return maxroom;
//     }
// };
```



### leetcode 254 数分解

```C++
class Solution {
public:
// 回溯比较慢
    vector<vector<int>> res;
    void dfs(int st, int en, vector<int>& ans)
    {
        if(en == 1)
        {
            if(ans.size() > 1) // 排除掉 
            {
               res.push_back(ans);
            }
            return ;
        }
        for(int i=st; i<=en; i++)
        {
            if(en%i == 0)
            {
                ans.push_back(i);
                dfs(i, en/i,ans);
                ans.pop_back();
            }
        }
        return ;
    }
    vector<vector<int>> getFactors(int n) {
        if(n==1)
        {
            return {};
        }
        vector<int>ans;
        dfs(2,n,ans);
        return res;
    }
};

// 
class Solution {
public:
    // l 代表因子的起始数，保证因子的有序性可以做到天然的去重
    vector<vector<int> > dfs(int n, int l) {
        vector<vector<int> > res;
        for (int i = l; i * i <= n; ++i) {
            if (n % i == 0) {
                res.push_back({ n / i, i });
                for (auto v : dfs(n / i, i)) {
                    v.push_back(i);
                    res.push_back(v);
                }
            }
        }
        return res;
    }
    vector<vector<int>> getFactors(int n) {
        return dfs(n, 2);
    }
};
```



```C++
binary_search(vector<int> nums, int start, int target) // 二分找target
{
	int left = start;
  int right = nums.size()-1;
  while(left < right)
  {
    int mid = left + (right- left)/2;
    if(nums[mid] < target)
    {
      left = mid;
    }
    else{
      right = mid -1;
    }
  }
  return left;
}

int twoSum(vector<int> nums, int start, int target)
{
  int sum = 0;
  int right = start;
  for(int i=start;i<nums.size() - 1 && nums[i]< target ; i++)
  {
    right = binary_search(nums, i, target- nums[i]);
    sum += right - i;
  }
  return sum;
}

int threeSum(vector<int> nums, int target)
{
  int sum = 0;
  for(int i=0; i < nums.size()-2 ; i++)
  {
    sum += twoSum(i+1, target - nums[i]);
  }
  return sum;
}


// 另一种双指针的方法
int twoSum1(vector<int> nums, int start,int target)
{
  int sum = 0;
  int left = start;
  int right = nums.size() - 1;
  while(left < right)
  {
    if(nums[left] + nums[right] < target)
    {
      sum += (right - left);
      left +=1;
    }
    else{
      right -- ;
    }    
  }
  return sum;
  
}

// 究极方法
class Solution {
public:
    int threeSumSmaller(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), res = 0;
        for(int i = 1; i < n - 1; i++){
            int num = 0;
            for(int lo = 0, hi = n - 1; i < hi && lo < i; ){
                if(nums[lo] + nums[i] + nums[hi] < target){
                    num = num + hi - i;
                    lo++;
                }
                else hi--;
            }
            res += num;
        }
        return res;
    }
};
```



### leetcode 261

注意有向图用拓扑排序，无向图用并查集

```C++
// 并查集

class Solution {
public:
    

    void union_node(vector<int>& parents,int a, int b)
    {
        int x = find(parents,a);
        int y = find(parents,b);
        if(x!=y)
        {
            parents[x] = y;
        }
        return ;
    }

    int find(vector<int>& parents,int a)
    {
        int r = a;
        int k = a;
        int j;
        while(r!=parents[r])
        {
            r = parents[r];
        }
        while(k!=r)
        {
            j = parents[k];
            parents[k] = r;
            k = j; 
        }
        return r;
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        vector<int>parents(n,0);
        for(int i =0 ;i<n;i++)
        {
            parents[i] = i;
        }
        for(int i=0;i<edges.size();i++)
        {
            int x = find(parents,edges[i][0]);
            int y = find(parents, edges[i][1]);
            if( x == y )
            {
                return false;
            }
            union_node(parents ,x , y);

        }
        // 判断 是不是只有一个连通
        int cnt = 0;
        for(int i = 0 ; i<n; i++)
        {
            if(i == parents[i])
            {
                cnt ++;
                if(cnt >= 2)
                {
                    return false;
                }
            }

        }
    return true;
    }
};

// 拓扑排序
class Solution {
public:
    // 判断是无环图 用栈？ 联通 拓扑排序 不知道问题在哪里
    bool validTree(int n, vector<vector<int>>& edges) {
        stack<int> st;
        vector<int> degrees(n,0);
        vector<int> res(n,0);
        for(auto x: edges)
        {
            degrees[x[1]] +=1;            
        }
        for(int i=0; i<n;i++)
        {
            if(degrees[i] == 0)
            {
                st.push(i);
            }
        }
        
        int cnt = 0;

        while(!st.empty())
        {
            int top = st.top();
            st.pop();
            res[cnt] = top;
            cnt++;
            for(int i=0; i<edges.size() ; i++)
            {
                if(edges[i][0] == top)
                {
                    degrees[edges[i][1]] -- ;
                    if(degrees[edges[i][1]] == 0)
                    {
                        st.push(edges[i][1]);
                    }
                }
                
            }
        }
        for(int i=0;i<n;i++)
        {
            cout<<res[i];
        }
        if(cnt < n)
        {
            return false;
        }
    return true;
    }
};
```



### leetcode 272 最近BST值

```C++
[1,null,2]  
3.428571
1
[1,null,8]
3.0
1
[4,2,5,1,3]
3.714286
2
[3,2,4,1]
2.000000
3
  
[5,3,6,1,4,null,null,null,2]
2.571429
1  
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    float fabs(float a)
    {
        return a>=0 ? a : -a;
    }

    vector<float>preorder(TreeNode* root)
    {
        if(!root) return {};
        vector<float> left = preorder(root->left);
        left.push_back(1.0*root->val);
        vector<float> right = preorder(root->right);
        for(auto x : right)
        {
            left.push_back(x);
        }
        return left;
    }
    vector<int> binary_search(vector<float>& li, float target, int k)
    {
        int left = 0;
        int right = li.size()-1;
        int mid = left + (right - left)/2;
        vector<int> ans;
        if(target >= li[right])
        {
            while(k--)
            {
                ans.push_back(right--);
            }
            return ans;
        }
        if(target <= li[left])
        {
            while(k--)
            {
                ans.push_back(left++);
            }
            return ans;
        }
        while(left < right)
        {
            mid = left + (right - left)/2;
            if(li[mid] > target)
            {
                right = mid -1 ;
            }
            else if(li[mid] < target){
                left = mid + 1;
            }else if(li[mid] == target){
                left = right = mid;
            }
        }
      	
        int center = left;
     		 if(li[left] > target)
        {
          center = left
        }
      	else if(li[right] > target)
        {
          center = right;
        }
      	else{
          center = li.size()-1;
        }
        //ans.push_back(center);
       // cout<<"center "<< center<< endl;
       if(li.size() == 2 && target < li[1] && target > li[0] )
        {
            center = (fabs(li[0] - target) < fabs(li[1] - target)) ? 0 : 1;
        }
        left = center;
        right = center+1;
        while(k--)
        {
            if(left >= 0 && right < li.size())
            {
                if( fabs(li[left] - target) < fabs(li[right] - target) )
                {
                    ans.push_back(left);  
                    left--;                    
                }
                else if(fabs(li[left] - target) > fabs(li[right] - target))
                {
                    ans.push_back(right);
                    right++;
                }
                else{
                    ans.push_back(left);
                    ans.push_back(right);
                    left--;
                    right++;
                }
            }
            else if(left<0 && right < li.size())
            {
                ans.push_back(right);
                right++;
            }
            else if(left >=0 && right >= li.size() )
            {
                ans.push_back(left);
                left -- ;
            }
        }
        return ans;
    }
    vector<int> closestKValues(TreeNode* root, double target, int k) {
        vector<float> preorderlist = preorder(root);
        int len = preorderlist.size();
        // for(auto x: preorderlist)
        // {
        //     cout << x; 
        // }
        vector<int> ans = binary_search(preorderlist, target, k);
        // vector<float> li(len, 0) ;
        // for(int i=0;i<preorderlist.size();i++)
        // {
        //     li[i] = abs(preorderlist[i]-target);
        // }
        for(int i=0;i<ans.size();i++)
        {
            ans[i] = int(preorderlist[ans[i]]);
        }
        return ans;

    }
};  
  

// 最近节点， 直接在树上查找 >= target
TreeNode* getSucc(TreeNode* root, TreeNode* target)
{
  if(root == nullptr)
  {
    return nullptr;
  }
  TreeNode* ans = nullptr;
  if(target == nullptr)
  {
    ans = root;
    while(ans->left)
    {
      ans = ans->left;
    }
    return ans;
  }
  if(target->right)
  {
    ans = target->right;
    while(ans->left)
    {
      
    }
  }
  
}

vector<int> closestKValues(TreeNode* root, double target, int k) {
 vector<int>ans;
  TreeNode* r = nullptr;
  TreeNode* cur = root;
  while(cur != nullptr && cur->val != target) // 找>= target
  {
    if(cur->val < target)
    {
      cur = cur->right;
    }else{
      r = cur;
      cur = cur->left;
    }
  }
	TreeNode* l = getPre(root, r); 
}  


/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> closestKValues(TreeNode* root, double target, int k) {
        vector<int> ans;
        if(root == nullptr){
            return ans;
        }

        TreeNode* r = nullptr;// >= target的最小节点
        TreeNode* cur = root;
        while(cur != nullptr && cur->val != target){
            if(cur->val < target){
                cur = cur->right;
            }else {
                r = cur;
                cur = cur->left;
            }
        }

        TreeNode* l = getPredecessor(root, r);
        while(ans.size() < k){
            if(l == nullptr){
                ans.push_back(r->val);
                r = getSuccessor(root, r);
            }else if(r == nullptr){
                ans.push_back(l->val);
                l = getPredecessor(root, l);
            }else{
                if(abs(l->val - target) < abs(r->val - target)){
                    ans.push_back(l->val);
                    l = getPredecessor(root, l);
                }else{
                    ans.push_back(r->val);
                    r = getSuccessor(root, r);
                }
            }
            // cout << ans.back() << endl;
        }
        
        return ans;
    }

    // 查找大于target的最小节点


    TreeNode* getSuccessor(TreeNode* root, TreeNode* target){
        
        if(root == nullptr){
            return nullptr;
        }

        TreeNode* ans = nullptr;
        // 空节点的后继是最小的节点
        if(target == nullptr){
            ans = root;
            while(ans->left){
                ans = ans->left;
            }
            return ans;
        }
        
        // 如果target有右子树，后继节点是右子树的最小值
        if(target->right){
            ans = target->right;
            while(ans->left){
                ans = ans->left;
            }
        }else{
            // 后继节点在root到target的路径上
            TreeNode* cur = root;
            while(cur != target){
                if(cur->val < target->val){
                    cur = cur->right;
                }else{
                    ans = cur;
                    cur = cur->left;
                }
            }
        }

        return ans;
    }

    TreeNode* getPredecessor(TreeNode* root, TreeNode* target){
        if(root == nullptr){
            return nullptr;
        }

        TreeNode* ans = nullptr;
        // 空节点的前驱是最大的节点
        if(target == nullptr){
            ans = root;
            while(ans->right){
                ans = ans->right;
            }

            return ans;
        }

        // 如果target有左子树，前驱节点是左子树的最大值
        if(target->left){
            ans = target->left;
            while(ans->right){
                ans = ans->right;
            }
        }else{
            // 前驱节点在root到target的路径上
            TreeNode* cur = root;
            while(target != cur){
                if(cur->val > target->val){
                    cur = cur->left;
                }else{
                    ans = cur;
                    cur = cur->right;
                }
            }
        }

        return ans;
    }
};
```



### leetcode 270 最近二叉搜索树

```C++
// 栈实现中序遍历，边遍历边找
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int closestValue(TreeNode* root, double target) {
		stack<TreeNode*>st;
      	float pre = -10e6;
  		while(!st.empty() ||  root)
        {
           while(root)
           {
             st.push(root);
             root = root-> left;          
           }
          root = st.top();
          st.pop();
          if(pre<= target && target < root->val)
          {
            return fabs(target-pre) > fabs(root->val - target) ? root->val : pre;
          }
          pre = root->val;
          root = root->right;
		}
      return pre;  // 移动到最右端了
    }  
};

class Solution {
  public int closestValue(TreeNode root, double target) {
    int val, closest = root.val;
    while (root != null) {
      val = root.val;
      closest = Math.abs(val - target) < Math.abs(closest - target) ? val : closest;
      root =  target < root.val ? root.left : root.right;
    }
    return closest;
  }
}


```





### leetcode 271 字符串编码

```C++
特殊用例
["0"]
["",""]
class Codec {
public:

  
    // Encodes a list of strings to a single string.
    string encode(vector<string>& strs) {
        string encodestr = "";
        int maxpos = 8;
        for(auto s: strs)
        {
            int len = s.length();
            int order = 0; // 位数
            int k = len;
            string number = "";
            while(k)
            {
                
                number += char('0'+ k%10 );
                k/=10;
                order++;
            }
            while(order < maxpos)
            {
                number+='0';
                order++;
            }
            reverse(number.begin(), number.end());
            encodestr+=(number+s);
        }
        return encodestr;
    }

    // Decodes a single string to a list of strings.
    vector<string> decode(string s) {
        vector<string> decodestr;
        int len = s.length();
        if(len < 8)
        {
            return {};
        }
        int i=0;
        while(i<len-1)
        {
            int slen = 0; 
            int k = 0;
            while(k < 8)
            {
                while(s[i] == '0' && k<8)
                {
                    i++;
                    k++;
                }
                while(s[i] >= '0' && s[i] <= '9' && k<8)
                {
                    slen*=10;
                    slen+= int(s[i] - '0');
                    i++;
                    k++;
                }
            }
            string str = "";
            for(int j =0 ; j < slen; i++,j++)
            {
                str+=s[i];
            }
            decodestr.push_back(str);       
        }
        return decodestr;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.decode(codec.encode(strs));
占用少内存 转换为16进制
const char HEX[16] =  {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'};
string int2Hex(int n)
{
  string res;
  while(n > 0)
  {
    res+=HEX[n%16];
    n/=16;
	}
  if(res.size()  < 4)
  {
    res.append(4 - res.size() , 0);
  }
  reverse(res.begin(), res.end());
  return res;
}

int hex2int(const string& s)
{
  int res = 0;
  for( auto x:s)
  {
    res<<=4;
    res |= (x<='9') ? (x - '0') : (c-'A' + 10);
	}
  return res;
}
		string encode(vector<string>& strs) {
        string res;
        for (auto& w : strs) {
            res += int2Hex(w.size());
            res += w;
        }
        return res;
    }

    // Decodes a single string to a list of strings.
    vector<string> decode(string s) {
        vector<string> res;
        int i = 0;
        int N = s.size();
        while (i < N) {
            int len = hex2Int(s.substr(i, 4));
            res.push_back(s.substr(i + 4, len));
            i += 4 + len;
        }
        return res;
    }
```

###  leetcode 276 栅栏涂色

```C++
dp
for(int i=1; i<n;i++)
{
  dp[i][0] = (k-1)*(dp[i-1][0] + dp[i-1][1]);
  dp[i-1][1] = dp[i-1][0];
}

人才
class Solution {
public:
    int numWays(int n, int k) {
        if(n == 1){
            return k;
        }
        int num_methods_of_former2_at_same_color = 0;
        int num_methods_of_former2_at_not_same_color = k;
        int ans;
        for(int i = 2; i <= n; ++i){
            ans = num_methods_of_former2_at_not_same_color * k + num_methods_of_former2_at_same_color * (k - 1);
            num_methods_of_former2_at_same_color = num_methods_of_former2_at_not_same_color;
            num_methods_of_former2_at_not_same_color = ans - num_methods_of_former2_at_same_color;
        }
        return ans;
    }
};  
```



### leetcode 277 搜寻名人

```C++
/* The knows API is defined for you.
      bool knows(int a, int b); */
// 注意 照着题解
class Solution {
public:
    int findCelebrity(int n) {
        int i = 0, j = 1;
        for (j = 1; j < n; j++) {
            if (knows(i, j)) {
                i = j;
            }
        }
        for (--j; j > i; j--) {
            if (!knows(j, i)) {
                return -1;
            }
        }
        for (--j; j >= 0; j--) {
            if (knows(i, j) || !knows(j, i)) {
                return -1;
            }
        }
        return i;
    }
};
```

### leetcode

```C++
["abc","ab"]

```



### leetcode 280

```C++
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
    bool less = true;
        for(int i=0; i<nums.size() -1; i++)
        {
        	if(less)
          {
            if(nums[i] > nums[i+1])
            {
              swap(nums[i],nums[i+1]);
            }
          }
          else{
            if(nums[i]< nums[i+1])
            {
              swap(nums[i+1], nums[i]);
            }
          }
          less = !less;
        }
    }
};
```



### leetcode 交叉迭代器

```C++

class ZigzagIterator1 {
private:
  int i,j, count = 0;
  vector<int>::iterator st1, st2, en1,en2;
public:
    ZigzagIterator1(vector<int>& v1, vector<int>& v2) {
			st1 = v1.begin();
      st2 = v2.begin();
      en1 = v1.end();
      en2 = v2.end();
    }
    int next(){
      assert(hasNext());
      int res;
      if(st1!=en1 && st2!=en2)
      {
        if(count%2 == 0)
        {
          res = *st1;
          st1++;
        }
        else{
          res = *st2;
          st2++;
        }
        count++;
        return res;
      }
      if(st1 != en1)
      {
        res = *st1;
        st1++;
        count++;
        return res;
			}
      else if(st2 != en2)
      {
        res = *st2;
        st2++;
        count++;
        return res;
			}else if(st1 == en1 && st2 == en2)
      {
        assert("No more elemnet");
      } 
    }
  bool hasNext(){
    
    return (st1!=en1) || (st2!=en2);
  }
  
  
  
  class ZigzagIterator1 {
public:
    ZigzagIterator1(vector<int>& v1, vector<int>& v2) {
        int i = 0;
        int j = 0;
        while (i < v1.size() && j < v2.size()) {
            nums.push_back(v1[i++]);
            nums.push_back(v2[j++]);
        }

        while (i < v1.size()) {
            nums.push_back(v1[i++]);
        }

        while (j < v2.size()) {
            nums.push_back(v2[j++]);
        }
    }

    int next() {
        assert(hasNext());
        return nums[index++];
    }

    bool hasNext() {
        return index < nums.size();
    }

private:
    int index = 0;
    vector<int> nums;
};

class ZigzagIterator2 {
public:
    ZigzagIterator2(vector<int>& v1, vector<int>& v2) : nums1(v1), nums2(v2) {
    }

    int next() {
        assert(hasNext());
        if (i < nums1.size() && j < nums2.size()) {
            if ((i + j) % 2 == 0) {
                return nums1[i++];
            } else {
                return nums2[j++];
            }
        } else if (i < nums1.size()) {
            return nums1[i++];
        } else if (j < nums2.size()) {
            return nums2[j++];
        } else {
            assert(false && "Unreachable!");
        }
    }

    bool hasNext() {
        return i < nums1.size() || j < nums2.size();
    }

private:
    int i = 0;
    int j = 0;
    vector<int> nums1;
    vector<int> nums2;
};


class ZigzagIterator3 {
public:
    ZigzagIterator3(vector<int>& v1, vector<int>& v2) {  
        i1 = v1.begin();
        i2 = v2.begin();
        e1 = v1.end();
        e2 = v2.end();
    }

    int next() {
        assert(hasNext());
        int res;
        if (i1 != e1 && i2 != e2)  {
            if (count % 2 == 0) {
                res = *i1++;
            } else {
                res = *i2++;
            }
            count++;
            return res;
        }
        
        if (i1 != e1) {
            res = *i1++; 
            count++;
            return res;
        } else if (i2 != e2) {
            res = *i2++; 
            count++;
            return res;
        } else {
            assert(false && "No more next!");
        }
    }

    bool hasNext() {
        return i1 != e1 || i2 != e2;
    }

private:
    int i;
    int j;
    int count = 0;
    vector<int>::iterator i1;
    vector<int>::iterator i2;
    vector<int>::iterator e1;
    vector<int>::iterator e2;
};

class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        if (!v1.empty()) {
            q.push(make_pair(v1.begin(), v1.end()));
        }

        if (!v2.empty()) {
            q.push(make_pair(v2.begin(), v2.end()));
        }
    }

    int next() {
        assert(hasNext());
        auto cur = q.front(); q.pop();
        if (cur.first + 1 != cur.second) {
            q.push(make_pair(cur.first + 1, cur.second));
        }

        return *cur.first;
    }

    bool hasNext() {
        return !q.empty();
    }

private:
    queue<pair<vector<int>::iterator, vector<int>::iterator>> q;
};

作者：jyj407
链接：https://leetcode-cn.com/problems/zigzag-iterator/solution/zhong-gui-zhong-ju-san-chong-jie-fa-zhu-b6xug/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



### leetcode 二叉搜索树中的顺序后继

```C++
// 两种情况， 针对没有右子树的情况，有两种方法， 分别需要O1和OH时间复杂度

class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
     	   TreeNode* res = null;
      		if(p->right)
          {
            p = p->right;
            while(p->left)
            {
              p = p->left;
            }
            return p;
          }
        // 如果没有右孩子, 从根节点找， 利用栈遍历找前驱和后继
        stack<TreeNode*> st;
        int prior = -1e9;
        while(root || !st.empty())
        {
          while(root)
          {
                st.push(root);
          			root = root->left;
          }
          root = st.top();
          st.pop();
          if(prior == p->val)
          {
            return root;
          }
          prior = root->val;
          root = root->right;         
        }
        return null;       
    }
 // 栈遍历中序
  1. 找左孩子，有则入栈，
  2. 无左孩子，左子树遍历完出栈
  3. 遍历右子树  
    
   void inorderTraversal(TreeNode *root){
    if(root==NULL)return;
    stack<TreeNode *>S;
    S.push(root);
    while(!S.empty()){
        //该过程一直找到没有左节点的节点才停止
        while(S.top()->left!=NULL){
            S.push(S.top()->left);
        }
        //此时的S.top()是一个没有left的节点，按照中序遍历的特性，可以将其直接输出。
        //while循环会一直将栈顶输出，直到遇到有右节点的节点，这样能保证栈中元素不会重复寻找左孩子
        while(!S.empty()){
            TreeNode *cur = S.top();
            cout<<cur>val<<" ";
            S.pop();
            
            if(cur>right!=NULL){
                S.push(cur->right);
                break;
            }
        }
    }
    cout<<endl;
   } 
  // another way 
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
     	   TreeNode* res = null;
       		if(p->right)
          {
            p = p->right;
            while(p->left)
            {
              p = p->left;
            }
            return p;
          }
      TreeNode* cur = root;
      while(cur!=p)
      {
        if(cur->val > p->val)
        {
          res = cur;
          cur = cur->left;
        }
        else{
          cur = cuir->right;
        }
      }
      return res;
    }
};
```

### leetcode 墙与门

```C++
// MLE 版
class Solution {
public:
    // BFS
    vector<vector<int>>dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    int isroom = 2147483647;
    int bfs(vector<vector<int>>& rooms, int row, int col)
    {
        int width = rooms.size();
        int height = rooms[0].size();
        vector<vector<int>>distance(width, vector<int>(height,0));

        pair<int, int> st(row, col);
        queue<pair<int, int>>q;
        q.push(st);
        while(!q.empty())
        {
            auto node = q.front();
            row = node.first;
            col = node.second;
            q.pop();
            for(int i=0;i<dirs.size(); i++)
            {
                int newrow = row + dirs[i][0];
                int newcol = col + dirs[i][1];
                if(newrow < 0 || newrow >= width || newcol < 0 || newcol >= height ||rooms[newrow][newcol]==-1 || distance[newrow][newcol])
                {
                    continue;
                } 
                distance[newrow][newcol] = distance[row][col]+1;
                if(rooms[newrow][newcol] == 0)
                {
                    return distance[newrow][newcol];
                }
                pair<int, int>newpair(newrow, newcol);
                q.push(newpair);              
            }
             
        }
        return rooms[row][col];
    }
    void wallsAndGates(vector<vector<int>>& rooms) {
        
        int width = rooms.size();
        int height = rooms[0].size();
        vector<vector<int>>ans(width, vector<int>(height,0));
        for(int i=0;i<width; i++)
        {
            for(int j=0;j<height;j++)
            {
                if(rooms[i][j] == isroom)
                {
                    rooms[i][j] = bfs(rooms, i,j);
                }
            }
        }
        return ;
    }
};



// 从门开始搜， 且把门一次性加入队列里
class Solution {
public:
    // BFS
    vector<vector<int>>dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    int isroom = 2147483647;
    int isgate = 0;
    int iswall = -1;
    void wallsAndGates(vector<vector<int>>& rooms) {
        
        int width = rooms.size();
        int height = rooms[0].size();
        vector<vector<int>>distance(width, vector<int>(height,0));
        queue<pair<int, int>>q;
        for(int i=0;i<width; i++)
        {
            for(int j=0;j<height;j++)
            {
                if(rooms[i][j] == isgate)
                {
                    pair<int, int> st(i, j);
                    q.push(st);
                }
            }
        }
        while(!q.empty())
        {
           auto node = q.front();
           q.pop();
            row = node.first;
            col = node.second;
            q.pop();
            for(int i=0;i<dirs.size(); i++)
            {
                int newrow = row + dirs[i][0];
                int newcol = col + dirs[i][1];
                if(newrow < 0 || newrow >= width || newcol < 0 || newcol >= height ||rooms[newrow][newcol] != isroom)
                {
                    continue;
                } 
                rooms[newrow][newcol] = rooms[row][col] + 1;
                q.push(pair<int, int>(i,j));
           }
        return ;
    }
};
  
  
  private static final int EMPTY = Integer.MAX_VALUE;
private static final int GATE = 0;
private static final List<int[]> DIRECTIONS = Arrays.asList(
        new int[] { 1,  0},
        new int[] {-1,  0},
        new int[] { 0,  1},
        new int[] { 0, -1}
);

public void wallsAndGates(int[][] rooms) {
    int m = rooms.length;
    if (m == 0) return;
    int n = rooms[0].length;
    Queue<int[]> q = new LinkedList<>();
    for (int row = 0; row < m; row++) {
        for (int col = 0; col < n; col++) {
            if (rooms[row][col] == GATE) {
                q.add(new int[] { row, col });
            }
        }
    }
    while (!q.isEmpty()) {
        int[] point = q.poll();
        int row = point[0];
        int col = point[1];
        for (int[] direction : DIRECTIONS) {
            int r = row + direction[0];
            int c = col + direction[1];
            if (r < 0 || c < 0 || r >= m || c >= n || rooms[r][c] != EMPTY) {
                continue;
            }
            rooms[r][c] = rooms[row][col] + 1;
            q.add(new int[] { r, c });
        }
    }
}
  
  
class Solution {
public:
    vector<vector<int>>dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    int isroom = 2147483647;
    int isgate = 0;
    int iswall = -1;
    void wallsAndGates(vector<vector<int>>& rooms) {
        
        int width = rooms.size();
        int height = rooms[0].size();
        vector<vector<int>>distance(width, vector<int>(height,0));
        queue<pair<int, int>>q;
        for(int i=0;i<width; i++)
        {
            for(int j=0;j<height;j++)
            {
                if(rooms[i][j] == isgate)
                {
                    pair<int, int> st(i, j);
                    q.push(st);
                }
            }
        }
        while(!q.empty())
        {
           auto node = q.front();
           int row = node.first;
           int col = node.second;
            q.pop();
            for(int i=0;i<dirs.size(); i++)
            {
                int newrow = row + dirs[i][0];
                int newcol = col + dirs[i][1];
                if(newrow < 0 || newrow >= width || newcol < 0 || newcol >= height ||rooms[newrow][newcol] != isroom)
                {
                    continue;
                } 
                rooms[newrow][newcol] = rooms[row][col] + 1;
                q.push(pair<int, int>(newrow, newcol));
           }
        return ;
    }
    }
};

  
  


```



### leetcode 127

```C++
class Solution {
public:
    unordered_map<string, int> wordId;
    vector<vector<int>> edge;
    int nodeNum = 0;

    void addWord(string& word) {
        if (!wordId.count(word)) {
            wordId[word] = nodeNum++;
            edge.emplace_back();
        }
    }

    void addEdge(string& word) {
        addWord(word);
        int id1 = wordId[word];
        for (char& it : word) {
            char tmp = it;
            it = '*';
            addWord(word);
            int id2 = wordId[word];
            edge[id1].push_back(id2);
            edge[id2].push_back(id1);
            it = tmp;
        }
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        for (string& word : wordList) {
            addEdge(word);
        }
        addEdge(beginWord);
        if (!wordId.count(endWord)) {
            return 0;
        }
        vector<int> dis(nodeNum, INT_MAX);
        int beginId = wordId[beginWord], endId = wordId[endWord];
        dis[beginId] = 0;

        queue<int> que;
        que.push(beginId);
        while (!que.empty()) {
            int x = que.front();
            que.pop();
            if (x == endId) {
                return dis[endId] / 2 + 1;
            }
            for (int& it : edge[x]) {
                if (dis[it] == INT_MAX) {
                    dis[it] = dis[x] + 1;
                    que.push(it);
                }
            }
        }
        return 0;
    }
};


// 找不到错误 存在hit -> h*t - > hot 直接跳变道 hit->*ot的问题
class Solution {
public:
    vector<vector<int>>edges; // 
    unordered_map<string, int>wordmap;
    int nodenum = 0;
    void addnode(string wd)
    {
        if(!wordmap.count(wd))
        {
            wordmap[wd] = nodenum++;
            edges.emplace_back();
        }
    }
    void addedge(string wd1, string wd2)
    {
        int id1 = wordmap[wd1];
        int id2 = wordmap[wd2];
        edges[id1].push_back(id2);
        edges[id2].push_back(id1);
    }
    void buildgraph(vector<string> wordList)
    {
        for(auto str : wordList)
        {
            auto s = str;
            addnode(str);
            char tmp = '*';
            for(int i=0;i<str.size(); i++)
            {
                char org = str[i];
                str[i] = '*';
                // char org = *ch;
                // *ch = tmp;
                addnode(str);
                addedge(str,s);
                str[i] = org;
            }
        }
        // for(int i=0;i<edges.size();i++)
        // {
        //     for(auto x: edges[i])
        //     {
        //         cout<< x;
        //     }
        //     cout<< endl;
        // }
        return ;
    }
    string findwd(int n)
    {
        for(auto it = wordmap.begin(); it!=wordmap.end(); it++)
        {
            if(it->second == n)
            {
                return it->first; 
            }
        }
        return "";
    }
    int bfs(string beginWord, string endWord)
    {
        vector<int>visited(nodenum,-1);
        vector<int>par(nodenum,-1);
        if(!wordmap.count(endWord))
        {
            return 0;
        }
        queue<int>q;
        q.push(wordmap[endWord]);
        visited[wordmap[endWord]] = 0;
        while(!q.empty())
        {
            int top = q.front();
            q.pop();
            if(top == wordmap[beginWord])
            {       
                cout<<par[0]<< endl;
                // cout<< findwd(top)<<" " ;
                // cout<< findwd(par[top]) << " ";
                // cout<< findwd(par[par[top]]) << " ";
                // int tmp = par[top];
                // while(tmp)
                // {
                //     cout<<findwd(tmp)<<" ";
                //     tmp = par[tmp];
                // }         
                return visited[top]/2 +1 ; 
            }
            for(auto x : edges[top])
            {
                if(visited[x] == -1)
                {
                    q.push(x);
                    par[x] = top;
                    visited[x] = visited[top] + 1;
                }
            }
        }
        return 0;
    }
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        buildgraph(wordList);
        for(auto it = wordmap.begin(); it!= wordmap.end(); it++)
        {
            cout<< it->first << it->second << endl;
        }
        //return 0;
        return bfs(beginWord, endWord);
    }
};
```



### leetcode288

```
["ValidWordAbbr","isUnique","isUnique"]
[[["door","naive"]],["cat"],["cat"]]

["ValidWordAbbr","isUnique"]
[[["a","a"]],["a"]]
```



### leetcode 296 

```C++
//找中位数
class Solution {
public:
    int distances(pair<int, int>st, pair<int, int> en)
    {
        return abs(st.first - en.first) + abs(st.second - en.second);
    }
    int minTotalDistance(vector<vector<int>>& grid) {
           vector<int> rows, cols;
           for(int i=0;i<grid.size(); i++)
           {
               for(int j=0; j<grid[0].size(); j++)
               {
                   if(grid[i][j] == 1)
                   {
                       rows.emplace_back(i);
                       cols.emplace_back(j);
                   }
               }
           }
           sort(rows.begin(), rows.end());
           sort(cols.begin(), cols.end());
           int n = rows.size();
           int m = n/2;
           int ans = 0;
           for(int i=0;i<m;i++)
           {
               ans+= (abs(rows[n-i-1] - rows[i]) + abs(cols[n-i-1] - cols[i] ) );
           }    
        return ans;
    }
};
```



### leetcode 29

```C++
// 两种方式， 自底向上或自顶向下
int maxlength;
int dfs(TreeNode* p)
{
  if(p == NULL)
  {
    return 0;
	}
  int left = dfs(p->left) + 1;
  int right = dfs(p->right) + 1;
  if(p->left!= NULL && p->val + 1 != p->left->val)
  {
    left = 1;
	}
  if(p->right != NULL && p->val +1 != p->right->val)
  {
    right = 1;
	}
  int length = max(left, right);
  maxlength = max(maxlength, length);
}

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode* child,TreeNode* father,int len)
    {
        if(child == NULL)
        {
            return len;
        }
        len = (father!= NULL  && child->val == father->val +1) ? len+1 : 1;
        return max(len, max( dfs(child->left, child, len), dfs(child->right,child, len) ));
    }
    int longestConsecutive(TreeNode* root) {
        return dfs(root, NULL, 1);
    }
};
```



### leetcode 

```C++
//dfs 搜索所有黑色点 
void dfs(char[][] image, int x, int y){
        if(x < 0 || y < 0 || x >= image.length || y >= image[0].length ||
          image[x][y] == '0')
            return;
        image[x][y] = '0'; // mark visited black pixel as white
        top = min(top, x);
        bottom = max(bottom, x + 1);
        left = min(left, y);
        right = max(right, y + 1);
        dfs(image, x + 1, y);
        dfs(image, x - 1, y);
        dfs(image, x, y - 1);
        dfs(image, x, y + 1);
    }
}

// 特殊方法 二分查找  投影到一维
class Solution {
public:
int searchrow(vector<vector<char>> image, int left, int right, int istop)
{
		while(left < right)
		{
			 int k = 0;
			 int mid = left + (right - left)/2;
			 while(k < image[0].size() && image[mid][k] == '0') k++;
            if(!istop)
            {
                if(k < image[0].size())  // 这一行有黑块
                {
                    right = mid;			
                }
                else{
                    left = mid + 1;
                }    
            }
        else{
                if(k >= image[0].size())  // 这一行有黑块
                {
                    right = mid;			
                }
                else{
                    left = mid + 1;
                }    
        }
    }
		return left;
}
int searchcol(vector<vector<char>> image, int left, int right, int istop)
{
		while(left < right)
		{
			 int k = 0;
			 int mid = left + (right - left)/2;
			 while(k < image.size() && image[k][mid] == '0') k++;
            if(!istop)
            {
                if(k < image.size())  // 这一列有黑块
                {
                    right = mid;			
                }
                else{
                    left = mid + 1;
                }    
            }
            else{
                if(k >= image.size())  // 这一列没有黑块
                {
                    right = mid;			
                }
                else{
                    left = mid + 1;
                }    
        }
    }
		return left;
}


    int minArea(vector<vector<char>>& image, int x, int y) {
        int left = searchcol(image, 0, y, false);
        int right = searchcol(image,y+1,image[0].size(), true);  // 左闭右开区间
        int low = searchrow(image,0,x,false);
        int high = searchrow(image, x+1, image.size(), true);
        // cout<<"left : "<< left << endl;
        // cout <<"right:  "<< right<< endl;
        // cout<<"low: "<< low <<endl;
        // cout<<"high: "<< high <<endl;
        return abs(right - left)* abs(high- low);
    }
};


public class Solution {
    public int minArea(char[][] image, int x, int y) {
        int m = image.length, n = image[0].length;
        int left = searchColumns(image, 0, y, 0, m, true);
        int right = searchColumns(image, y + 1, n, 0, m, false);
        int top = searchRows(image, 0, x, left, right, true);
        int bottom = searchRows(image, x + 1, m, left, right, false);
        return (right - left) * (bottom - top);
    }
    private int searchColumns(char[][] image, int i, int j, int top, int bottom, boolean whiteToBlack) {
        while (i != j) {
            int k = top, mid = (i + j) / 2;
            while (k < bottom && image[k][mid] == '0') ++k;
            if (k < bottom == whiteToBlack) // k < bottom means the column mid has black pixel
                j = mid; //search the boundary in the smaller half
            else
                i = mid + 1; //search the boundary in the greater half
        }
        return i;
    }
    private int searchRows(char[][] image, int i, int j, int left, int right, boolean whiteToBlack) {
        while (i != j) {
            int k = left, mid = (i + j) / 2;
            while (k < right && image[mid][k] == '0') ++k;
            if (k < right == whiteToBlack) // k < right means the row mid has black pixel
                j = mid;
            else
                i = mid + 1;
        }
        return i;
    }
}

```



### leetcode 29 翻转游戏

博弈论内容， 利用SG 函数

本质上是NIM游戏的扩展， 利用异或说明先手必胜还是后手必胜



### leetcode 305 岛屿游戏

```C++
// 并查集优化  注意写法 yyds
#include <iostream>
#include <vector>

using namespace std;

class UnionFind {
public:
    UnionFind(int N) {
        for (int i = 0; i < N; ++i) {
            parent.push_back(i);
        }
        count = 0;
    }

    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }

    int getCount() {
        return count;
    }

    void addCount() {
        count++;
    }

    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    void unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return;
        }
        
        parent[rootX] = rootY;
        count--;
    }

    int getCount() const {
        return count;
    }

private:
    vector<int> parent;
    int count;
};

class Solution {
private:
    bool inArea(int x, int y, int m, int n) {
        return x >= 0 && x < m && y >= 0 && y < n;
    }

public:
    vector<int> numIslands2(int m, int n, vector<vector<int>> &positions) {
        UnionFind unionFind(m * n);
        vector<bool> visited;
        for (size_t i = 0; i < m * n; i++) {
            visited.push_back(false);
        }

        int DIRECTIONS[4][2] = {{0,  1},
                                {1,  0},
                                {0,  -1},
                                {-1, 0}};
        vector<int> res;
        for (auto &position : positions) {
            int currX = position[0];
            int currY = position[1];

            int index = currX * n + currY;
            if (!visited[index]) {
                // 把水变成陆地，连通分量个数加 1
                unionFind.addCount();
                visited[index] = true;
                for (auto &direction : DIRECTIONS) {
                    int newX = currX + direction[0];
                    int newY = currY + direction[1];
                    int newIndex = newX * n + newY;
                    // 下面这三个条件很重要
                    if (inArea(newX, newY, m, n)
                        && visited[newIndex]
                        && !unionFind.isConnected(index, newIndex)) {
                        unionFind.unionSet(index, newIndex);
                    }
                }
            }
            res.push_back(unionFind.getCount());
        }
        return res;
    }
};
```



### leetcode308 二维区域

几种方式都可以，  可以考虑线段树方法

```C++
class NumMatrix {
public:
    vector<vector<int>>sumarr;
    vector<vector<int>> matrix;
    NumMatrix(vector<vector<int>>& matrix): matrix(matrix) {
      sumarr = vector<vector<int>>(matrix.size(), vector<int>(matrix[0].size(),0) );
        for(int i=0;i<matrix.size(); i++)
        {
            sumarr[i][0] = matrix[i][0];
            for(int j=1;j<matrix[0].size(); j++)
            {
                sumarr[i][j] = sumarr[i][j-1] + matrix[i][j];
            }
        }
    }
    
    void update(int row, int col, int val) {
        int det = val - matrix[row][col] ;
        matrix[row][col] = val;     
        for(int j=col; j<matrix[0].size();j++)
        {
            sumarr[row][j] = sumarr[row][j] + det; 
        }
        
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        int ans = 0;
        for(int i=row1; i<=row2; i++)
        {
            ans+= sumarr[i][col2] - sumarr[i][col1] + matrix[i][col1];
        }
        return ans;
    }
};



/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * obj->update(row,col,val);
 * int param_2 = obj->sumRegion(row1,col1,row2,col2);
 */
```



### leetcode 314 二叉树垂直

层次遍历 设置节点

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
// 采用这篇文章的方法： 
public:
    // 找左孩子的右子树和右孩子的左子树
    vector<vector<int>> verticalOrder(TreeNode* root) {
        if(root == NULL)
        {
            return {};
        }
        vector<vector<int>>ans;
        map<int, vector<int>> vertical;
        queue<pair<TreeNode* , int>>q;
        q.push(make_pair(root, 0));
        vertical[0].push_back(root->val);
        while(!q.empty())
        {
            int level = q.size();
            while(level -- )
            {
                pair<TreeNode*, int> top = q.front();
               
                q.pop();
                if(top.first->left)
                {
                    int x = top.second - 1;
                    vertical[x].push_back(top.first->left->val);
                    q.push(make_pair(top.first->left, x));
                }
                if(top.first->right)
                {
                    int x = top.second + 1;
                    vertical[x].push_back(top.first->right->val);
                    q.push(make_pair(top.first->right, x));
                }
            }
        }
      for(auto it = vertical.begin(); it!=vertical.end(); it++)
      {
          ans.push_back(it->second);
      }
        return ans;
    }
};

// dalao 方法

class Solution {
public:
    map<int, vector<pair<int, int>>> m;
    vector<vector<int>> verticalOrder(TreeNode* root) {
        dfs(root, 0, 0);
        vector<vector<int>> result;
        for (auto& [k, v]: m) {
            auto cmp = [](const pair<int, int>& p1, const pair<int, int>& p2) {
                return p1.second < p2.second;
            };
            sort(v.begin(), v.end(), cmp);
            vector<int> tmp;
            for (auto& p: v) {
                tmp.push_back(p.first);
            }
            result.push_back(tmp);
        }
        return result;
    }
    void dfs(TreeNode* root, int col, int depth) {
        if (!root) {
            return;
        }
        m[col].push_back({root->val, depth});
        dfs(root->left, col - 1, depth + 1);
        dfs(root->right, col + 1, depth + 1);
    }
};
```

### leetcode 317 离建筑物最近距离

技巧 利用空地编号

```C++
class Solution {
public:
    int shortestDistance(vector<vector<int>>& grid) {
    	//从每个建筑物出发去找空地，空地对每个建筑物的距离累积
    	vector<vector<int>> dir = {{1,0},{0,1},{0,-1},{-1,0}};
    	vector<vector<int>> build;
    	int i, j, k, x, y, mindis;
    	int m = grid.size(), n = grid[0].size();
    	for(i = 0; i < m; i++)
    		for(j = 0; j < n; j++)
    			if(grid[i][j]==1)
    				build.push_back({i,j});
    	vector<vector<int>> dis(m, vector<int>(n, 0));
    	//记录单次遍历1个房屋到空地的距离
    	vector<vector<int>> totaldis(m, vector<int>(n, 0));
    	//记录所有房屋到空地的距离
    	int emptyPlace = 0;//空地的标记数字
    	for(auto& pos : build)//遍历房子
    	{
    		queue<vector<int>> q;
    		// vector<vector<bool>> visited(m, vector<bool>(n,false));
    		q.push({pos[0], pos[1]});//x,y
    		mindis = INT_MAX;
    		while(!q.empty())
    		{
                x = q.front()[0];
                y = q.front()[1];
                q.pop();
                for(k = 0; k < 4; ++k)
                {
                    i = x + dir[k][0];
                    j = y + dir[k][1];
                    if(i>=0 && i<m && j>=0 && j<n && 
                    		grid[i][j] == emptyPlace)//空地
                    {
                        dis[i][j] = dis[x][y]+1;//该房子到ij空地的距离
                        totaldis[i][j] += dis[i][j];//之前所有房子到这的距离和
                        mindis = min(mindis, totaldis[i][j]);
                        // visited[i][j] = true;
                        grid[i][j]--;//从lx_lx大佬那学的，不用开访问数组了
                        q.push({i,j});
                    }
                }
    		}
            if(mindis == INT_MAX)
                return -1;//该房屋不能到达任何空地
            emptyPlace--;//更新空地的标记，免开访问标记数组
    	}
    	return mindis==INT_MAX ? -1 : mindis;
    }
};


```



### leetcode 320 单词缩写

两种情况

1.保留下一个字符

2. 缩写下一个字符

```C++
class Solution {
public:
    vector<string> ans;
    string int2str(int n)
    {
        string s = "";
        while(n)
        {
            s+=char(n%10 + '0');
            n/=10;
        }
        reverse(s.begin(), s.end());
        return s;
    }
    void dfs(string word,string tmp, int n, int k)  // n: 当前到第几个字符 // 当前的缩写个数
    {
        if(n == word.length())
        {
          if(k!=0) tmp+= int2str(k);
          ans.push_back(tmp);
          return ;
        }
        dfs(word, tmp, n+1, k+1);
        if(k!=0)
        {
          tmp+=int2str(k);
        }
        tmp+=word[n];
        dfs(word, tmp, n+1, 0);       
   }
    vector<string> generateAbbreviations(string word) {
        dfs(word,"", 0,0);
       return ans;
    }
};
```



### leetcode 246 中心对称数

```C++
class Solution {
public:
    bool isStrobogrammatic(string num) {
        int len = num.length();
        string old_num = num;
        for(int i=0; i<len; i++)
        {
           switch(num[i] - '0'){
           case 1: num[i] = '1'; break;
           case 0: num[i] = '0'; break;
           case 8: num[i] = '8'; break;
           case 6: num[i] = '9'; break;
           case 9: num[i] = '6'; break;
           default: return false; 

           }

        }
        int left = 0;
        int right = len-1;
        // cout<< num;
        for(int i=0; left<=right && i<len;i++)
        {
            swap(num[left], num[right]);
            left++;
            right--;
            if(num[i] != old_num[i])
            {
                return false;
            }
        }
        return true;
    }
};
```

### leetcode 247 中心2

```C++
class Solution {
public:
    vector<string> helper(int n,bool first=false) {
        if (n == 0){
            return vector<string>{""};
        }
        if (n == 1) {
            return vector<string>{"0", "1", "8"};
        }
        vector<string> subs = helper(n-2);
        vector<string> ans;
        for (auto sub:subs) {
            if(!first){
                ans.push_back("0"+sub+"0");
            }
            ans.push_back("1"+sub+"1");
            ans.push_back("6"+sub+"9");
            ans.push_back("8"+sub+"8");
            ans.push_back("9"+sub+"6");
        }
        return ans;
    }
    vector<string> findStrobogrammatic(int n) {
        return helper(n,true);
    }
};
```



### leetcode 展开二维向量

```C++
class Vector2D {
public:
  vector<vector<int>>v ;
  int inner = 0;
  int outer = 0;
  Vector2D(vector<vector<int>>& vec)
  {
    	v = vec;
	}
  void advanceToNext()
  {
    	while(outer<v.size() && inner == v[outer].size())  // 用while 避免中间出现空数组的情况
      {
         inner = 0；
         outer++;
			}
  }
  int next()
  {
    	if(!hasNext()) assert(!hasNext());
      return v[outer][inner++];
	}
  bool hasNext()
  {
    advanceToNext();
    return outer < v.size();
  }

};
```



### leetcode 284 认识迭代器

```C++
class PeekingIterator : public Iterator {
public:
    PeekingIterator(const vector<int>& nums) : Iterator(nums) {
        flag = Iterator::hasNext();
        if (flag) {
            nextElement = Iterator::next();
        }
    }
    
    int peek() {
        return nextElement;
    }
    
    int next() {
        int ret = nextElement;
        flag = Iterator::hasNext();
        if (flag) {
            nextElement = Iterator::next();
        }
        return ret;
    }
    
    bool hasNext() const {
        return flag;
    }
private:
    int nextElement;
    bool flag;
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/peeking-iterator/solution/ding-duan-die-dai-qi-by-leetcode-solutio-8toa/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  
  template <class T>
class PeekingIterator : public Iterator<T> {
public:
    PeekingIterator(const vector<T>& nums) : Iterator<T>(nums) {
        flag = Iterator<T>::hasNext();
        if (flag) {
            nextElement = Iterator<T>::next();
        }
    }
    
    T peek() {
        return nextElement;
    }

    T next() {
        T ret = nextElement;
        flag = Iterator<T>::hasNext();
        if (flag) {
            nextElement = Iterator<T>::next();
        }
        return ret;
    }
    
    bool hasNext() const {
        return flag;
    }
private:
    T nextElement;
    bool flag;
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/peeking-iterator/solution/ding-duan-die-dai-qi-by-leetcode-solutio-8toa/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



### leetcode 连通分量， 并查集板子

### leetcode325

前缀和应用， 在最长等差数列， 几个数之和都用到

```C++
class Solution {
public:
    int maxSubArrayLen(vector<int>& nums, int k) {
        int sum_ = 0;
        unordered_map<int, int>map1;
        map1[0] = 0; // 这行很关键，出现子数组sum和为0，保证最长
        int res = 0;
        for(int i=0;i<nums.size(); i++)
        {
            sum_ += nums[i];
            if(map1.count(sum_) == 0)
            {
                map1[sum_] = i + 1;
            }
            if(map1.count(sum_ - k))
            {
                res = max(res, i - map1[sum_ - k] + 1);
            }
        }
    return res;
     }
};
```



### leetcode 333 最大bst

```C++
// 	方法1 暴力dfs 查看 对每个节点检验一次 
class Solution{
public:
	bool valid(TreeNode* root, int l, int r) // 区间思想 检查是否落在一个区间
	{
		if(root == NULL)
		{
			return true;
		}
		if(root->val <= l || root->val >=r)
    {
      return false;
    }
    return valid(root->left, l, root->val) && valid(root->right, root->val, r);
	}
  int cnt(TreeNode* root)
  {
     return root ? cnt(root->left)+cnt(root->right) +1 : 0;
  }
	int largestBSTSubtree(TreeNode* root)
  {
    if(root == NULL)return 0;
    if (valid(root,INT_MIN,INT_MAX)) return cnt(root);
        return max(largestBSTSubtree(root->left),largestBSTSubtree(root->right));
  }
}

// 错误用例 [3,null,1,2,null,null,4,null,5]
// 为啥这种方法居然比上面的慢
class Solution{
  int ans = 0;
public:
  struct Node{
		int l , r , sz;
  };
  Node dfs(TreeNode* root)
  {
    	int valid = 1;
    	if(root->left == NULL && root->right == NULL)
      {
        ans = max(ans,1);
        return (Node){root->val, root->val,1};  // 注意这种写法
      }
      int l= root->val, r= root->val, sz = 0; 
    	if(root->left)
      {
        Node L = dfs(root->left);
        if(L.sz!=-1 && root->val > L.r)
        {
           sz += L.sz;
           l = L.l;
        }
        else{
          valid = 0;
        }
      }
			if(root->right)
      {
        	Node R =dfs(root->right);
        	if(R.sz != -1 && root->val < R.l)
          {
             sz += R.sz;
             r = R.r; 
					}
        	else{
            valid = 0;
          }
			}
    if(valid)
    {
      	ans = max(ans,sz+1);
      	return (Node){l,r,sz+1};
    }
    return (Node){-1,-1,-1};
  }
  int largestBSTSubtree(TreeNode root) {
        if(root == null){
            return 0;
        }
        dfs(root);
        return ans;       
    } 
}
```



### leetcode 339 嵌套列表求和

```C++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    int sum = 0;
    void dfs(vector<NestedInteger> nestedList, int cnt)
    {   
        
            for(int i=0;i<nestedList.size(); i++)
            {
                if(nestedList[i].isInteger())
                {
                    sum += cnt*nestedList[i].getInteger();
                }
                else{
                    dfs(nestedList[i].getList(), cnt+1);
                }
            }
        
        return ;      
    }
    int depthSum(vector<NestedInteger>& nestedList) {
        dfs(nestedList, 1);
        return sum;
    }
};
```



### leetcode [至多包含 K 个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)

```C++
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        int len = s.length();
        if(!(len && k) )
        {
            return 0;
        }
        unordered_map<char, int>map1;
        unordered_map<char, int>:: iterator it0;
        int ans =1;
        int left = 0, right = 0;
        while(right < len)
        {
            map1[s[right]] = right ;
            right++;
            if(map1.size() == k+1)
            {
                int left_most = INT_MAX;
                for(auto it= map1.begin(); it!= map1.end(); it++)
                {
                    if(it->second < left_most)
                    {
                        left_most = it->second;
                        it0 = it;
                    }
                }
                map1.erase(it0);
                left = left_most + 1;
            } 
            ans = max(ans, right - left);
        }
        return ans;
    }
};
```



### leetcode [348. 设计井字棋](https://leetcode-cn.com/problems/design-tic-tac-toe/)

```C++
class TicTacToe {
public:
    vector<vector<int>> r, c, d;
    int n;
    TicTacToe(int n) {
        this->n = n;
        r = c = vector<vector<int>>(3, vector<int>(n));
        d = vector<vector<int>>(3, vector<int>(3, 0));
    }
    
    int move(int row, int col, int player) {
        if (++r[player][row] == n) {
            return player;
        }
        if (++c[player][col] == n) {
            return player;
        }
        if (row == col && ++d[player][1] == n) {
            return player;
        }
        if (row + col == n - 1 && ++d[player][2] == n) {
            return player;
        }
        return 0;
    }
};


// 另一种牛逼方法  做差分就行
class TicTacToe {
public:
    TicTacToe(int n) : sz(n) {
        rows.resize(n, 0), cols.resize(n, 0);
        diagonal = anti_diagonal = 0;
    }
    
    int move(int row, int col, int player) {
        if (player == 1) {
            ++rows[row], ++cols[col];
            if (row == col)
                ++diagonal;
            if (row == sz - 1 - col)
                ++anti_diagonal;
            if (rows[row] == sz || cols[col] == sz || diagonal == sz || anti_diagonal == sz)
                return 1;
        }
        else {
            --rows[row], --cols[col];
            if (row == col)
                --diagonal;
            if (row == sz - 1 - col)
                --anti_diagonal;
            if (rows[row] == -sz || cols[col] == -sz || diagonal == -sz || anti_diagonal == -sz)
                return 2;
        }
        return 0;
    }

private:
    vector<int> rows, cols;
    int diagonal, anti_diagonal;
    int sz;
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe* obj = new TicTacToe(n);
 * int param_1 = obj->move(row,col,player);
 */
/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe* obj = new TicTacToe(n);
 * int param_1 = obj->move(row,col,player);
 */

作者：huo-hu
链接：https://leetcode-cn.com/problems/design-tic-tac-toe/solution/cyi-ban-shi-xian-by-huo-hu-1436/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



### leetcode [351. 安卓系统手势解锁](https://leetcode-cn.com/problems/android-unlock-patterns/)

```C++
// 注意虽然可以重复经过节点，但在回溯过程中不用考虑重复，因为构图上节点只有一个
bool visited[9];
int numberOfPatterns(int m, int n)
{
  int res = 0;
  for(int len = m; len <=n; len++)
  {
    res += dfs(-1,len);
    for(int i=0; i<9; i++)
    {
      visited[i] = false;
    }
  }
  return res;
}
bool check(int st, int en)
{
   if(visited[st])
   {
     return false;
   }
   if(en == -1)
   {
     return true;
   }
   if( (st+en)%2 == 1 )
   {
   	  return true; 
   }
   int mid = (st + en)/2;
   if(mid == 4)
   {
     return visited[mid];
   }
   if((st%3 != en%3) && (st/3 != en/3))
   {
     return true;
   }
  return visited[mid];
}
int dfs(int start, int len)
{
   if(len == 0)
   {
     return 1;
   }
  int sum = 0;
  for(int i=0;i<9; i++)
  {
     if(check(start,i) && start!=i)
     {
        visited[i] = true;
        sum += dfs(i, len -1 );
        visited[i] = false;
     }
  }
  return sum;
}

}
```



### leetcode [353. 贪吃蛇](https://leetcode-cn.com/problems/design-snake-game/)

```C++
class SnakeGame {
public:
    int w,h;
    int curx=0, cury=0;
    int curlen = 1;
    int food_index = 0;
    int score = 0;
    vector<vector<int>> f;
    deque<pair<int, int>> snake;  // 用deque模拟蛇身范围
    unordered_set<int> mem; // 思路： 用set记录蛇身覆盖的位置
    bool check(int x, int y)
    {
        if(x<0 || x>=h || y<0 || y>=w)
        {
            return false;
        }
        if(mem.count(x*w+y) && (x*w+y)!=(snake.back().first*w + snake.back().second) ) // 注意尾部 最后一个节点不算
        {
            return false;
        }
        return true;
    }
    SnakeGame(int width, int height, vector<vector<int>>& food) {
        w = width;
        h = height;
        f = food;
        curx = 0;
        cury = 0;
        mem.insert(0);
        snake.push_front(make_pair(0,0));
    }
    
    int move(string direction) {
        // switch(direction):
        // case "U": cury--; break;
        // case  "R":  break;  注意C++里 string不能用作 case选项
        if(direction == "U")
        {
            curx--;
        }
        if(direction == "R")
        {
            cury++;
        }
        if(direction == "D")
        {
            curx++;
        }
        if(direction == "L")
        {
            cury--;
        }
        if(!check(curx, cury))
        {
            return -1;
        }
        if(food_index < f.size() && curx == f[food_index][0] && cury == f[food_index][1])
        {
            score++;
            food_index++;
        }
        else{ // 当蛇吃到食物后蛇身变长
          
             // 有bug 直接这么写old1.first 的结果是一个随机值
             mem.erase(snake.back().first*w + snake.back().second);
             snake.pop_back();
        }
      
        snake.push_front(make_pair(curx, cury));
        mem.insert(curx*w+cury);
       
        return score;
    }
};

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame* obj = new SnakeGame(width, height, food);
 * int param_1 = obj->move(direction);
 */
```



### leetcode [346. 数据流中的移动平均值](https://leetcode-cn.com/problems/moving-average-from-data-stream/)

```C++
class MovingAverage {
    int head;
    int tail;
    int size;
    int count;
    long sum=0;
    vector<int> num;
public:
    MovingAverage(int size) 
    {
        head=-1;
        tail=-1;
        count =0;
        this->size =size;
        num.resize(size);
    }
    double next(int val) 
    {
        if(count <size)
        {
            ++count;
            ++tail;
            num[tail]=val;
            sum+=val;
        }
        else
        {   
            head = (++head)%size;
            sum-=num[head];

            tail = (++tail)%size;
            num[tail]=val; 
            sum+=val;
        }
        return sum*1.0/count;
    }
    

};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```



### leetcode[356. 直线镜像](https://leetcode-cn.com/problems/line-reflection/)

```
class Solution 
{
public:
    bool isReflected(vector<vector<int>>& points) 
    {
        int n = points.size();
        int L = INT_MAX;
        int R = INT_MIN;
        set<pair<int,int>> p_set;   //pair只能set
        for (auto & p: points)
        {
            L = min(L, p[0]);
            R = max(R, p[0]);
            p_set.insert(pair<int,int>{p[0], p[1]});
        }

        double mid_x = ((double)L + R) / 2;
        int cnt = 0;
        for (auto [x1, y] : p_set)
        {
            int x2 = 2 * mid_x - x1;
            if ( p_set.find(pair<int,int>{x2, y}) == p_set.end() )
                return false;
            cnt ++;
            if (cnt > n / 2)    //比较一半就可以了。因为set是有序的
                break;
        }
        return true;
    }
};

作者：Hanxin_Hanxin
链接：https://leetcode-cn.com/problems/line-reflection/solution/cpython3-zhao-zhong-xian-bian-li-yan-zhe-x9mn/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

