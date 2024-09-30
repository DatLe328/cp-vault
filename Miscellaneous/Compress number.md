Use for solving problem are not depending on original value like distinct value
https://cses.fi/problemset/task/1734/
```cpp
void compress() {
    map<int, int> mp;
    for (int i = 1; i <= n; i++) {
        mp[a[i]] = i;
    }
    for (int i = 1; i <= n; i++) {
        a[i] = mp[a[i]];
    }
}
```