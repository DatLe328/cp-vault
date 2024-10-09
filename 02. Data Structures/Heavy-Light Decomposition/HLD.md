#tree #graph
```cpp
const int MAX_N = 2e5 + 1;
 
int depth[MAX_N],   // Node's depth
	pos[MAX_N],     // Node's position in array
	val[MAX_N],     // Array's value
	sz[MAX_N],      // Parent's size
	par[MAX_N],     // Child's parent
    chain_id[MAX_N];// Node's chain id
vector<int> adj[MAX_N];
int n, q;
 
void update(int idx, int v) {
	// Implement update function
}
 
int query(int l, int r) {
	// Implement query function
}
 
void dfs(int u, int p) {
	sz[u] = 1;
	par[u] = p;
	for (int v : adj[u]) {
		if (v == p) continue;
		depth[v] = depth[u] + 1;
        dfs(v, u);
        sz[u] += sz[v];
	}
}
int cur_pos = 1;
void hld(int u, int par, int top) {
	pos[u] = cur_pos++;
	chain_id[u] = top;
	update(pos[u], val[u]);
	int h_v = -1, h_sz = -1;
	for (int v : adj[u]) {
		if (v == par) continue;
		if (sz[v] > h_sz) {
			h_sz = sz[v];
			h_v = v;
		}
	}
	if (h_v == -1) return;
	hld(h_v, u, top);
	for (int v : adj[u]) {
		if (v == par || v == h_v) continue;
		hld(v, u, v);
	}
}
 
int path(int x, int y) {
	int ret = 0;
	while (chain_id[x] != chain_id[y]) {
		if (depth[chain_id[x]] < depth[chain_id[y]]) swap(x, y);
		ret = max(ret, query(pos[chain_id[x]], pos[x]));
		x = par[chain_id[x]];
	}
	if (depth[x] > depth[y]) swap(x, y);
	ret = max(ret, query(pos[x], pos[y]));
	return ret;
}
```