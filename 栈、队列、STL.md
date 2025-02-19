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
上述代码时间复杂度O(n*n),在数据量很大时会出现超时。
