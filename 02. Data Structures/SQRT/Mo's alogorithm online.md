https://codeforces.com/problemset/problem/940/F
```cpp
#include <iostream>
#include <stdio.h>
#include <algorithm>
#include <cmath>
#include <cstring>
#include <vector>
using namespace std;

const int MAX_N = 2e5 + 6;
const int MAX_M = 2e5 + 6;

int cnt[MAX_M];
int s[MAX_N];

struct Query {
    int L,R,Lid,Rid,T,id;
    void give_val(int _L,int _R,int _Lid,int _Rid,int _T,int _id) {
        L = _L;
        R = _R;
        Lid = _Lid;
        Rid = _Rid;
        T = _T;
        id = _id;
    }
    bool operator<(const Query &q2) {
        if (Lid != q2.Lid) return Lid < q2.Lid;
        if (Rid != q2.Rid) return Rid < q2.Rid;
        return T < q2.T;
    }
} query[MAX_N];

struct Modify {
    int pos,ori_val,after_val;
    void give_val(int _pos,int _ori_val,int _after_val) {
        pos = _pos;
        ori_val = _ori_val;
        after_val = _after_val;
    }
} modify[MAX_N];

int ans[MAX_N];

const int G = 450;

int hhave[G+1];

void add(int x) {
    if (cnt[x] <= 0 || cnt[x] > G)
    {
        ;
    }
    else
    {
        hhave[ cnt[x] ]--;
    }
    cnt[x]++;
    if (cnt[x] <= G)
    {
        hhave[ cnt[x] ]++;
    }
}

void sub(int x) {
    if (cnt[x] <= G && cnt[x] >= 0)
    {
        hhave[ cnt[x] ]--;
    }
    cnt[x]--;
    if (cnt[x] <= 0 || cnt[x] > G)
    {
        ;
    }
    else
    {
        hhave[ cnt[x] ]++;
    }
}

void addTime(int T,int L,int R) {
    if (L <= modify[T].pos && modify[T].pos <= R) {
        sub(s[modify[T].pos]);
        add(modify[T].after_val);
    }
    s[modify[T].pos] = modify[T].after_val;
}

void subTime(int T,int L,int R) {
    if (L <= modify[T].pos && modify[T].pos <= R) {
        sub(s[modify[T].pos]);
        add(modify[T].ori_val);
    }
    s[modify[T].pos] = modify[T].ori_val;
}

struct Tm
{
    int a,b,c;
    Tm(){}
    Tm(int _a,int _b,int _c):a(_a),b(_b),c(_c) {}
} tttttt[MAX_N];

int main () {
    int n,m;
    scanf("%d %d",&n,&m);
    memset(ans,-1,sizeof(ans));
    vector<int> v;
    for (int i=1;n>=i;++i) {
        scanf("%d",&s[i]);
        v.push_back(s[i]);
    }
    int T=-1;
    int qid=0;
    int B = pow(max(n,m),2.0/3.0);
    if (B<=0) B=1;
    for (int i=1;m>=i;++i)
    {
        int a,b,c;
        scanf("%d %d %d",&a,&b,&c);
        if (a == 2)
        {
            v.push_back(c);
        }
        tttttt[i] = Tm(a,b,c);
    }
    sort(v.begin(),v.end());
    v.resize(unique(v.begin(),v.end()) - v.begin());
    for (int i=1;n>=i;i++)
    {
        s[i] = lower_bound(v.begin(),v.end(),s[i]) - v.begin()+1;
    }
    for (int i=1;m>=i;++i) {
        int t = tttttt[i].a;
        int x = tttttt[i].b,y = tttttt[i].c;
        if (t == 1) {
            query[++qid].give_val(x,y,x/B,y/B,T,i);
        }
        else if (t==2){
            y = lower_bound(v.begin(),v.end(),y) - v.begin() + 1;
            modify[++T].give_val(x,s[x],y);
            s[x] = y;
        }
    }
    sort(query+1,query+qid+1);
    int L=1,R=0;
    for (int i=1;qid >= i;++i) {
        while (query[i].R > R) add(s[++R]);
        while (query[i].L < L) add(s[--L]);
        while (query[i].R < R) sub(s[R--]);
        while (query[i].L > L) sub(s[L++]);
        while (query[i].T > T) addTime(++T,L,R);
        while (query[i].T < T) subTime(T--,L,R);
        int _=1;
        while (hhave[_] > 0)
        {
            ++_;
        }
        ans[query[i].id] = _;
    }
    for (int i=1;m>=i;++i) {
        if (ans[i] != -1) printf("%d\n",ans[i]);
    }
}

```