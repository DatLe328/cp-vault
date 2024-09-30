```cpp
#include<bits/stdc++.h>
using namespace std;
const int MaxN = 2e5 + 5;

int N, Q;
int Val[MaxN];
vector<int> AdjList[MaxN]; // input

int Par[MaxN]; // parent
int Depth[MaxN]; // do sau cua node
int Sz[MaxN]; // kich thuoc cua cay con cho cac node
int Pos[MaxN]; // vi tri trong mang cua node
int Arr[MaxN]; // gia tri cua cac phan tu trong mang
int ChainID[MaxN]; // ChainID[i]: Chain ma i nam trong
int ChainHead[MaxN]; // ChainHead[i]: Node dau tien trong chain i
int CurChain, CurPos;

void Dfs(int s, int p = -1) {
    Sz[s] = 1;
    for(int u: AdjList[s]) {
        if(u == p) continue;
        Par[u] = s;
        Depth[u] = Depth[s] + 1;
        Dfs(u, s);
        Sz[s] += Sz[u];
    }
}

void Hld(int s, int p = -1) {
    if(!ChainHead[CurChain]) {
        ChainHead[CurChain] = s;
    }
    ChainID[s] = CurChain;
    Pos[s] = CurPos;
    Arr[CurPos] = s;
    CurPos++;
    int nxt = 0;
    for(int u: AdjList[s]) {
        if(u != p) {
            if(nxt == 0 || Sz[u] > Sz[nxt]) nxt = u;
        }
    }
    if(nxt) Hld(nxt, s);
    for(int u: AdjList[s]) {
        if(u != p && u != nxt) {
            CurChain++;
            Hld(u, s);
        }
    }
}

// find LCA

int LCA(int u, int v) {
    while(ChainID[u] != ChainID[v]) {
        if(ChainID[u] > ChainID[v]) {
            u = Par[ChainHead[ChainID[u]]];
        }
        else {
            v = Par[ChainHead[ChainID[v]]];
        }
    }
    if(Depth[u] < Depth[v]) return u;
    return v;
}

// Segment Tree

int64_t ST[MaxN * 4];
void Build(int id, int l, int r) {
    if(l == r) {
        ST[id] = Val[Arr[l]];
        return;
    }
    int mid = (l + r) / 2;
    Build(id * 2, l, mid);
    Build(id * 2 + 1, mid + 1, r);
    ST[id] = ST[id * 2] + ST[id * 2 + 1];
}

void Upd(int id, int l, int r, int pos, int val) {
    if (l > pos || r < pos) return;
    if (l == r && l == pos) {
        ST[id] = val;
        return;
    }
    int mid = (l + r) / 2;
    Upd(id * 2, l, mid, pos, val);
    Upd(id * 2 + 1, mid + 1, r, pos, val);
    ST[id] = ST[id * 2] + ST[id * 2 + 1];
}

int64_t Calc(int id, int tl, int tr, int l, int r) {
    if (tl > r || tr < l) return 0;
    if (l <= tl && tr <= r) return ST[id];
    int mid = (tl + tr) / 2;
    return Calc(id * 2, tl, mid, l, r) + Calc(id * 2 + 1, mid + 1, tr, l, r);
}

// Update and queries

void Update(int x, int val) {
    Upd(1, 1, N, Pos[x], val);
}

int64_t Query(int u, int v) {
    int lca = LCA(u, v);
    int64_t ans = 0;
    while(ChainID[u] != ChainID[lca]) {
        ans += Calc(1, 1, N, Pos[ChainHead[ChainID[u]]], Pos[u]);
        u = Par[ChainHead[ChainID[u]]];
    }
    while(ChainID[v] != ChainID[lca]) {
        ans += Calc(1, 1, N, Pos[ChainHead[ChainID[v]]], Pos[v]);
        v = Par[ChainHead[ChainID[v]]];
    }
    if(Depth[u] < Depth[v]) {
        ans += Calc(1, 1, N, Pos[u], Pos[v]);
    }
    else {
        ans += Calc(1, 1, N, Pos[v], Pos[u]);
    }
    return ans;
}

// main

signed main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    cin >> N >> Q;
    for(int i = 1; i <= N; i++) {
        cin >> Val[i];
    }
    for(int i = 1; i < N; i++) {
        int u, v;
        cin >> u >> v;
        AdjList[u].push_back(v);
        AdjList[v].push_back(u);
    }
    CurPos = CurChain = 1;
    Dfs(1);
    Hld(1);
    Build(1, 1, N);
    while(Q--) {
        int type;
        cin >> type;
        if(type == 1) {
            // Update
            int x, val;
            cin >> x >> val;
            Update(x, val);
        }
        else {
            int u;	cin >> u;
            cout << Query(1, u) << '\n';
        }
    }
}

```