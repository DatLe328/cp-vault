https://cses.fi/problemset/task/1734/
```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
const int d = 500;
int a[MAX_N], n, q, dict = 0;
int buc[MAX_N], ans[MAX_N];
 
struct Query {
    int id, l, r;
    Query() {}
    Query(int id, int l, int r) : id(id), l(l), r(r) {}
 
    bool operator< (Query b) {
        Query a = *this;
        return make_pair(a.l / d, a.r) < make_pair(b.l / d, b.r);
    }
};
 
Query qs[MAX_N];
 
void add(int idx) {
    if (buc[a[idx]] == 0) {
        dict++;
    }
    buc[a[idx]]++;
}
 
void remove(int idx) {
    if (buc[a[idx]] == 1) {
        dict--;
    }
    buc[a[idx]]--;
}
void compress() {
    map<int, int> mp;
    for (int i = 1; i <= n; i++) {
        mp[a[i]] = i;
    }
    for (int i = 1; i <= n; i++) {
        a[i] = mp[a[i]];
    }
}
 
void MoAlgorithm() {
    sort(qs + 1, qs + q + 1);
    int curL = 1, curR = 1;
    add(1);
    for (int i = 1; i <= q; i++) {
        int l = qs[i].l, r = qs[i].r;
        while (curL > l) { add(--curL); }
        while (curL < l) { remove(curL++); }
        while (curR > r) { remove(curR--); }
        while (curR < r) { add(++curR); }
        ans[qs[i].id] = dict;
    }
}
 
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    compress();
    for (int i = 1; i <= q; i++) {
        int l, r;   cin >> l >> r;
        qs[i].id = i;
        qs[i].l = l, qs[i].r = r;
    }   
    MoAlgorithm();
    for (int i = 1; i <= q; i++) {
        cout << ans[i] << '\n';
    }
 
    return 0;
}
```