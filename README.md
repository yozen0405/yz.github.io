# yz.github.io
## 題意

- $\texttt{tree}$ 加上幾個 $\texttt{edge}$ 可以變雙聯通 (沒有 $\texttt{bridge}$ )

## 想法

### 答案

- 觀察可發現我們需要連的是 $\texttt{leaf}$

- 如果有 $k$ 個 $\texttt{leaf}$，需要再加 $\ge k/2$ 個 $\texttt{edge}$

<img src="https://i.imgur.com/T9t3xKU.jpg" width="400" />

- 以上圖來說，我們只需要 $6/2=3$ 個 $\texttt{edge}$ 就可以把它連通

- 兩兩 $\texttt{leaf}$ 連線剛好可以產生**環**，我們使環跟環之間有交集，就可以把圖連通

- 如果 $k$ 是奇數，就把最後一個  $\texttt{leaf}$ 連到第 $1$ 個
- 這邊所說的第 $x$ 個是以他們的 $\texttt{timestamp}$ 順序來編號的
- 我們絕對不能讓 $\texttt{root}$ 的邊只有一個，不然怎麼連都沒用
  - 如果圖只有兩個點那特判，其他 $\texttt{case}$ 都需要使 $\texttt{root}$ 的邊數量不為 $1$

### 證明
![](https://i.imgur.com/ogtXWhD.png)

- 如果是以這種方式連那代表 $[1..10]$ 的子樹包含 $[5..6]$ ，也代表 $[5..6]$ 很可能跟 $[1..10]$ 沒有交集，那只要斷他們之間的邊整張圖就會斷掉，代表此種方式不可行
- 我們需要找到一種方式使得環跟環之間有交集

![](https://i.imgur.com/23j6RHz.png)



> 證明 $[1..6],[2..7]$ 的 $\texttt{path}$ 一定有交集

- 因為 $[1..6]$ 會形成一個環， $2$ 在環內 $7$ 在環外，不管怎麼連都一定會經果環
  - 所以 $\texttt{path}$ 有交集
 
![](https://i.imgur.com/kjtAdux.png)

只有兩種 $\texttt{case}$

- $x$ 是 $y$ 的祖先
- $y$ 是 $x$ 的祖先

> $\texttt{LCA(1,6),LCA(2,7)}$ 比較靠近的 $\texttt{root}$ 就是 $\texttt{LCA(1,7)}$

- 我們就假設 $x$ 是 $y$ 的祖先

- 因為 $y$ 是 $2,7$ 的祖先
- $x$ 是 $1,6$ 的祖先
- 又 $x$ 是 $y$ 的祖先所以 $x$ 是 $1,6,2,7$ 的祖先，也就是 $x=\texttt{LCA(1,6,2,7)}$
- 又因為其實 $[1,7]$ 這兩個 $\texttt{node}$ 的 $\texttt{timestamp}$ 完全包住 $[2,6]$ 這兩個 $\texttt{node}$
  - $[2,6]$ 為 $[1,7]$ 的 $\texttt{subtree}$
  - $\texttt{LCA(1,7)}$ 為 $\texttt{LCA(2,6)}$ 的祖先
- 所以代表 $\texttt{LCA(1,6,2,7)=LCA(1,7)}$ (因為 $\texttt{LCA}$ 要看的其實是他們全部共同的，如果有一個比較高要選高的)
  - $x=\texttt{LCA(1,6,2,7)=LCA(1,7)}$
- 有此可知 $\texttt{path}$ 之間有交集能讓他們所覆蓋的 $\texttt{node}$ 越來越上去
- 這種連法可以使任兩個環跟環之間有交集，具體連法參考下面的構造 $\texttt{section}$

<img src="https://i.imgur.com/BnRxeoQ.jpg" width="300" />

### 構造
- 先將每個 $\texttt{leaf}$ 用 $\texttt{timestamp}$ 做編號
- 假設 $k$ 個 $\texttt{leaf}$
- 再將 $\texttt{leaf}$ 以 $i$ 跟 $i+k/2$ 連

  - 因為這樣在環內的 $\texttt{leaf}$ 有 $i+1,..,i+(k/2)-1$
  - 剛好有包含一些左邊的起始點跟右邊的一些結束點
  - 剛好每一個環都有至少包到一個結束點或起始點
  - 代表環與環之間都有交集，做法成立

## CODE

```cpp
#include <bits/stdc++.h>
#define int long long
#define pii pair<int, int>
#define pb push_back
#define mk make_pair
#define F first
#define S second
#define ALL(x) x.begin(), x.end()

using namespace std;
 
const int INF = 2e18;
const int maxn = 3e5 + 5;
const int M = 1e9 + 7;

vector<int> G[maxn];
int n;
vector<int> leaf;

void dfs (int u, int par) {
    if (G[u].size() == 1) leaf.pb(u);
    for (auto v : G[u]) {
        if (v == par) continue;
        dfs (v, u);
    }
}

void init() {
    cin >> n;
    int u, v;
    for (int i = 0; i < n - 1; i++) {
        cin >> u >> v;
        G[u].pb(v);
        G[v].pb(u);
    }
}

void solve() {
    dfs (1, 0);
    if (leaf.size() & 1) leaf.pb(leaf[0]); // 使最後一個連到第一個

    cout << leaf.size()/2 << "\n";

    for (int i = 0; i < leaf.size()/2; i++) {
        cout << leaf[i] << " " << leaf[i + leaf.size()/2] << "\n";
    }
} 
 
signed main() {
    // ios::sync_with_stdio(0);
    // cin.tie(0);
    int t = 1;
    //cin >> t;
    while (t--) {
        init();
        solve();
    }
} 
```

  

  

  
