```cpp
class SuffixArray {
private:
    int n;
    string s;
    vector<int> p, c;
    void count_sort(vector<int>& p, vector<int>& c) {
        int n = p.size();
        vector<int> cnt(n);
        for (auto x : c) {
            cnt[x]++;
        }
        vector<int> p_new(n);
        vector<int> pos(n);
        pos[0] = 0;
        for (int i = 1; i < n; i++) {
            pos[i] = pos[i - 1] + cnt[i - 1];
        }
        for (auto x : p) {
            int i = c[x];
            p_new[pos[i]] = x;
            pos[i]++;
        }
        p = p_new;
    }
public:
    SuffixArray(string str) : s(str) {
        s += "$";
        this->n = s.size();
        p.resize(n), c.resize(n);
        {
            vector<pair<char, int>> a(n);
            for (int i = 0; i < n; i++) {
                a[i] = {s[i], i};
            }
            sort(a.begin(), a.end());
            for (int i = 0; i < n; i++) {
                p[i] = a[i].second;
            }
            c[p[0]] = 0;
            for (int i = 1; i < n; i++) {
                if (a[i].first == a[i - 1].first) {
                    c[p[i]] = c[p[i - 1]];
                }
                else {
                    c[p[i]] = c[p[i - 1]] + 1;
                }
            }
        }
        int k = 0;
        while ((1 << k) < n) {
            for (int i = 0; i < n; i++) {
                p[i] = (p[i] - (1 << k) + n) % n;
            }
            count_sort(p, c);

            vector<int> c_new(n);
            c_new[p[0]] = 0;
            for (int i = 1; i < n; i++) {
                pair<int, int> prev = {c[p[i - 1]], c[(p[i - 1] + (1 << k)) % n]};
                pair<int, int> now = {c[p[i]], c[(p[i] + (1 << k)) % n]};
                if (now == prev) {
                    c_new[p[i]] = c_new[p[i - 1]];
                }
                else {
                    c_new[p[i]] = c_new[p[i - 1]] + 1;
                }
            }
            c = c_new;
            k++;
        }
    };
    bool find(string res) {
        int m = res.size();
        int l = 1, r = n - 1;
        int left = -1, right = -1;
        while (l <= r) {
            int mid = (l + r) / 2;
            int cur = min(m, n - p[mid] - 1);
            string tmp = s.substr(p[mid], cur);
            if (res == tmp) {
                left = mid;
                return true;
            }
            else if (res > tmp) {
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }
        return false;
    }
};

```