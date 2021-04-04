# Treap

$Treap=Tree(BST)+Heap$

能维护的：

1. 插入 x*x* 数
2. 删除 x*x* 数(若有多个相同的数，因只删除一个)
3. 查询 x*x* 数的排名(排名定义为比当前数小的数的个数 +1 )
4. 查询排名为 x*x* 的数
5. 求 x*x* 的前驱(前驱定义为小于 x*x*，且最大的数)
6. 求 x*x* 的后继(后继定义为大于 x*x*，且最小的数)

# P3369 【模板】普通平衡树

```cpp
// Problem: P3369 【模板】普通平衡树
// Contest: Luogu
// URL: https://www.luogu.com.cn/problem/P3369#sub
// Memory Limit: 128 MB
// Time Limit: 1000 ms
// Date: 2021-02-25 15:47:20
// --------by Herio--------

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull; 
const int N=1e5+5,M=2e4+5,inf=0x3f3f3f3f,mod=1e9+7;
#define mst(a,b) memset(a,b,sizeof a)
#define PII pair<int,int>
#define fi first
#define se second
#define pb emplace_back 
void Print(int *a,int n){
	for(int i=1;i<n;i++)
		printf("%d ",a[i]);
	printf("%d\n",a[n]); 
}
int sum=0,R=0;//sum (sum of node) R (root index)
int size[N],v[N],num[N],rd[N],son[N][2];
inline void pushup(int p){
    size[p]=size[son[p][0]]+size[son[p][1]]+num[p];
}
inline void rotate(int &p,int d){	//d=0 (left rotate)
    int k=son[p][d^1];
    son[p][d^1]=son[k][d];
    son[k][d]=p;
    pushup(p),pushup(k),p=k;
}
void ins(int &p,int x){
    if (!p){
        p=++sum,size[p]=num[p]=1;
        v[p]=x,rd[p]=rand();
        return;
    }
    if (v[p]==x){
        num[p]++,size[p]++;return;
    }
    int d=(x>v[p]);
    ins(son[p][d],x);
    if (rd[p]<rd[son[p][d]]) rotate(p,d^1);//Max Heap
    pushup(p);
}
void del(int &p,int x){
    if (!p) return;
    if(x!=v[p]) del(son[p][x>v[p]],x);
    else{
        if (!son[p][0] && !son[p][1]){
            num[p]--,size[p]--; 
            if (!num[p]) p=0;
        } 
        else if (son[p][0] && !son[p][1]){
            rotate(p,1);
            del(son[p][1],x);
        }
        else if (!son[p][0] && son[p][1]){
            rotate(p,0);
            del(son[p][0],x);
        }
        else if (son[p][0] && son[p][1]){
            int d=(rd[son[p][0]]>rd[son[p][1]]);
            rotate(p,d);
            del(son[p][d],x);
        }
    }
    pushup(p);
}
int rk(int p,int x){
    if (!p) return 0;
    if (v[p]==x) return size[son[p][0]]+1;
    if (v[p]<x) return size[son[p][0]]+num[p]+rk(son[p][1],x);
    if (v[p]>x) return rk(son[p][0],x);
}
int find(int p,int x){
    if (!p) return 0;
    if (size[son[p][0]]>=x) return find(son[p][0],x);
    else if (size[son[p][0]]+num[p]<x)
        return find(son[p][1],x-num[p]-size[son[p][0]]);
    else return v[p];
}
int pre(int p,int x){
    if (!p) return -inf;
    if (v[p]>=x) return pre(son[p][0],x);
    else return max(v[p],pre(son[p][1],x));
}
int suc(int p,int x){
    if (!p) return inf;
    if (v[p]<=x) return suc(son[p][1],x);
    else return min(v[p],suc(son[p][0],x));
}
#define getchar()(p1==p2&&(p2=(p1=buf)+fread(buf,1,1<<21,stdin),p1==p2)?EOF:*p1++)
char buf[1<<21],*p1=buf,*p2=buf;
template <typename T>
inline T& read(T& r) {
    r = 0; bool w = 0; char ch = getchar();
    while(ch < '0' || ch > '9') w = ch == '-' ? 1 : 0, ch = getchar();
    while(ch >= '0' && ch <= '9') r = r * 10 + (ch ^ 48), ch = getchar();
    return r = w ? -r : r;
}
int main(){
    int n;read(n);
    for (int i=0;i<n;++i){
        int opt,x;read(opt),read(x);
        if (opt==1) ins(R,x);
        else if (opt==2) del(R,x);
        else if (opt==3) printf("%d\n",rk(R,x));
        else if (opt==4) printf("%d\n",find(R,x));
        else if (opt==5) printf("%d\n",pre(R,x));
        else if (opt==6) printf("%d\n",suc(R,x));
    }
    return 0;
}
```

---

有旋 Treap 常数小

无选fhq Treap 常数较大。

---

https://www.luogu.com.cn/record/47117035