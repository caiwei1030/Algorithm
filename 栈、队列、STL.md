# 牛客竞赛语法入门班数组栈、队列和stl习题<br>
#### 练习题J
https://ac.nowcoder.com/acm/contest/19850/L
```
#include<bits/stdc++.h>
using namespace std;


int main(){
    // 关闭同步，使得cin和cout不再与C的stdio同步，提高效率
    ios::sync_with_stdio(false);
    // 解除cin和cout的绑定，进一步提高输入输出效率
    cin.tie(NULL);
    cout.tie(NULL);
    int m,k;
    cin>>m>>k;
    //记录指纹锁内容
    unordered_set<int> zhiwen;
    string opt;
    int x;
    for(int i=0;i<m;i++){
        cin>>opt>>x;
        if(opt=="add"){
            if(zhiwen.size()==0){
                zhiwen.insert(x);
            }else{
                bool flag=false;//表示是否有与其相似指纹
                for(int a:zhiwen){
                    if(abs(a-x)<=k){
                        flag=true;
                        break;
                    }
                }
                if(!flag)zhiwen.insert(x);
            }
        }
        if(opt=="del"){
            vector<int> tmp;
            //注意：使用迭代器时不可直接删除元素
            for(int a:zhiwen){
                if(abs(a-x)<=k){
                    tmp.push_back(a);
                }
            }
            for(int t:tmp){
                zhiwen.erase(t);
            }
        }
        if(opt=="query"){
            bool ison=false;
            for(int a:zhiwen){
                if(abs(a-x)<=k){
                    cout<<"Yes"<<endl;
                    ison=true;
                    break;
                }
            }
            if(ison==false)cout<<"No"<<endl;
        }
    }
    return 0;
}
```
对于每个操作（add、query、del），需要遍历整个集合来判断是否存在与 x 差值不超过 k 的元素，导致每个操作的时间复杂度是 O(n)，在最坏情况下 m 个操作整体复杂度可能达到 O(m·n)（甚至近似 O(m²)）。

### set
set 内部通常采用`平衡二叉搜索树（如红黑树)`实现，这使得所有存入的元素都会按顺序排列。
<br>优势：
这样可以利用`二分查找`的思想快速定位目标元素，降低搜索时间到 O(log n)。
<br>`set中常用的函数`
begin()     　　 ,返回set容器的第一个元素

end() 　　　　 ,返回set容器的最后一个元素

clear()   　　     ,删除set容器中的所有的元素

empty() 　　　,判断set容器是否为空

max_size() 　 ,返回set容器可能包含的元素最大个数

size() 　　　　 ,返回当前set容器中的元素个数

rbegin　　　　 ,返回的值和end()相同

rend()　　　　 ,返回的值和rbegin()相同
<br>`用 lower_bound 实现范围查找`
对于 add 和 query 操作，需要判断集合中是否存在一个元素 a 使得
$∣a−x∣≤k⇔a∈[x−k,x+k]$
<br>实现方式：
利用set 的 lower_bound(x-k)，可以快速获得第一个不小于 (x-k) 的元素。
如果这个元素存在且其值不大于 (x+k)，就说明集合中存在满足条件的指纹。
否则就不存在满足条件的指纹，从而可以执行插入或其他操作。
