# Google コーディング面接 3週間学習計画
**前提:** LeetCode初心者 → Google面接通過レベル（面接日: 2026-07-16）
**言語: Python 3**

---

## 全体戦略

3週間でゼロから仕上げるには「広く浅く」ではなく「頻出パターンを深く」が正解。
Googleが実際に出す問題の80%は以下5パターンに収束する：

```
① 配列・文字列（Two Pointer / Sliding Window / HashMap）
② 木・グラフ（BFS / DFS / 再帰）
③ 動的計画法（メモ化 / ボトムアップ）
④ ヒープ・優先度キュー（Top-K問題）
⑤ 二分探索
```

**週次目標:**
- Week 1: パターン習得（解法の型を覚える）
- Week 2: 応用と定着（Medium問題を自力で解けるようにする）
- Week 3: 面接シミュレーション（本番形式で解く）

**1日の時間配分（2〜3時間想定）:**
```
[45分] 新しいパターン学習 or 動画解説を見る
[60分] LeetCode問題を解く（2〜3問）
[30分] 解けなかった問題の解説を読み、コードを写経
[15分] 昨日解いた問題を見直す（間隔反復）
```

---

## Python 面接用チートシート（常時参照）

### データ構造の使い分け

```python
# ---- リスト（配列・スタック） ----
arr = [1, 2, 3]
arr.append(x)        # O(1) 末尾追加
arr.pop()            # O(1) 末尾削除
arr.pop(0)           # O(n) 先頭削除 ← 遅いので注意
arr.insert(0, x)     # O(n) 先頭挿入 ← 遅い

# ---- deque（キュー・両端キュー） ----
from collections import deque
q = deque()
q.append(x)          # O(1) 右追加
q.appendleft(x)      # O(1) 左追加
q.pop()              # O(1) 右削除
q.popleft()          # O(1) 左削除  ← BFSで必須

# ---- ハッシュマップ ----
from collections import defaultdict, Counter
d = defaultdict(int)      # キーなしでも d[key] += 1 できる
d = defaultdict(list)     # d[key].append(v) できる
c = Counter("aabbc")      # {'a':2, 'b':2, 'c':1}
c.most_common(k)          # 頻度上位k個

# ---- ヒープ（最小ヒープ） ----
import heapq
heap = []
heapq.heappush(heap, val)
heapq.heappop(heap)       # 最小値を取り出す
heapq.heapify(list)       # O(n) でリストをヒープに変換

# 最大ヒープは値を負にして使う
heapq.heappush(heap, -val)
max_val = -heapq.heappop(heap)

# ---- セット ----
s = set()
s.add(x)             # O(1)
s.discard(x)         # O(1) ※存在しなくてもエラーにならない
x in s               # O(1) 検索
```

---

### 頻出イディオム（面接で書くと好印象）

```python
# ソート：キー指定
intervals.sort(key=lambda x: x[0])         # 区間の左端でソート
words.sort(key=lambda w: (len(w), w))       # 長さ → 辞書順

# 無限大
float('inf')   # 正の無限大
float('-inf')  # 負の無限大

# enumerate（インデックスと値を同時に）
for i, val in enumerate(arr):
    pass

# zip（複数リストを同時に）
for a, b in zip(list1, list2):
    pass

# リスト内包表記
matrix = [[0] * cols for _ in range(rows)]  # 2次元配列の初期化
                                              # ※ [[0]*cols]*rows はNG（参照共有バグ）

# 文字列 ↔ リスト変換
chars = list("hello")        # ['h','e','l','l','o']
"".join(chars)               # "hello"
s.split()                    # スペースで分割
",".join(["a","b","c"])      # "a,b,c"

# 辞書のget（デフォルト値付き）
count = d.get(key, 0)

# 条件式（三項演算子）
val = a if condition else b

# 多重代入（スワップ）
a, b = b, a                  # 一時変数不要

# 文字コード変換
ord('a')    # → 97
chr(97)     # → 'a'
ord(c) - ord('a')  # アルファベットを0-25の数字に変換
```

---

### パターン別 Python テンプレート（面接で即使える）

```python
# ===== Two Pointer =====
left, right = 0, len(arr) - 1
while left < right:
    if arr[left] + arr[right] == target:
        return [left, right]
    elif arr[left] + arr[right] < target:
        left += 1
    else:
        right -= 1

# ===== Sliding Window（可変長）=====
from collections import defaultdict
left = 0
window = defaultdict(int)
result = 0
for right in range(len(s)):
    window[s[right]] += 1
    while len(window) > k:          # 条件が破れたら縮める
        window[s[left]] -= 1
        if window[s[left]] == 0:
            del window[s[left]]
        left += 1
    result = max(result, right - left + 1)

# ===== Binary Search =====
left, right = 0, len(nums) - 1
while left <= right:
    mid = (left + right) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
return -1

# ===== BFS（グラフ / 最短路）=====
from collections import deque
def bfs(start):
    queue = deque([start])
    visited = {start}
    steps = 0
    while queue:
        for _ in range(len(queue)):   # レベル単位で処理
            node = queue.popleft()
            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        steps += 1
    return steps

# ===== DFS（再帰）=====
def dfs(node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor, visited)

# ===== DFS（スタック・非再帰）=====
stack = [start]
visited = {start}
while stack:
    node = stack.pop()
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            stack.append(neighbor)

# ===== DP（1次元）=====
dp = [0] * (n + 1)
dp[0] = base
for i in range(1, n + 1):
    dp[i] = dp[i-1] + dp[i-2]  # 遷移式はケースによる

# ===== バックトラッキング =====
def backtrack(start, path):
    result.append(path[:])     # コピーを追加（重要）
    for i in range(start, len(candidates)):
        path.append(candidates[i])
        backtrack(i + 1, path)
        path.pop()             # 元に戻す

# ===== ヒープ（Top-K）=====
import heapq
# K番目に大きい要素
heap = []
for num in nums:
    heapq.heappush(heap, num)
    if len(heap) > k:
        heapq.heappop(heap)     # 最小値を捨てる → 残りがTop-K
return heap[0]                  # ヒープの先頭がK番目に大きい値
```

---

### Python での計算量の注意点

| 操作 | 計算量 | 注意 |
|---|---|---|
| `list.append(x)` | O(1) amortized | — |
| `list.pop(0)` / `list.insert(0,x)` | O(n) | **deque を使う** |
| `x in list` | O(n) | **set を使う** |
| `x in set` / `x in dict` | O(1) | — |
| `sorted()` | O(n log n) | — |
| `"".join(list)` | O(n) | `+=` で連結を繰り返すのは O(n²) なので **join を使う** |
| スライス `arr[l:r]` | O(r-l) | コピーが発生することに注意 |

---

## Week 1（6/25〜7/1）: パターン習得期

**目標:** 基本パターン5種類の「型」を体に叩き込む。解法を見ながらでも良い。

### Day 1（6/25 水）| 配列 — Two Pointer

| 問題 | 難易度 | リンク |
|---|---|---|
| Two Sum | Easy | [#1](https://leetcode.com/problems/two-sum/) |
| Valid Palindrome | Easy | [#125](https://leetcode.com/problems/valid-palindrome/) |
| 3Sum | Medium | [#15](https://leetcode.com/problems/3sum/) |

**学ぶパターン:**
```python
# Two Pointer の型
left, right = 0, len(arr) - 1
while left < right:
    if condition:
        left += 1
    else:
        right -= 1
```

---

### Day 2（6/26 木）| 配列 — Sliding Window

| 問題 | 難易度 | リンク |
|---|---|---|
| Best Time to Buy and Sell Stock | Easy | [#121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |
| Longest Substring Without Repeating Characters | Medium | [#3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| Minimum Window Substring | Hard | [#76](https://leetcode.com/problems/minimum-window-substring/)（解説を読むだけでOK） |

**学ぶパターン:**
```python
# Sliding Window の型
left = 0
for right in range(len(s)):
    # windowに right の要素を追加
    while window_invalid:
        # windowから left の要素を削除
        left += 1
    # 最大/最小を更新
```

---

### Day 3（6/27 金）| ハッシュマップ & 文字列

| 問題 | 難易度 | リンク |
|---|---|---|
| Valid Anagram | Easy | [#242](https://leetcode.com/problems/valid-anagram/) |
| Group Anagrams | Medium | [#49](https://leetcode.com/problems/group-anagrams/) |
| Top K Frequent Elements | Medium | [#347](https://leetcode.com/problems/top-k-frequent-elements/) |

---

### Day 4（6/28 土）| 木 — DFS & BFS ★重要★

| 問題 | 難易度 | リンク |
|---|---|---|
| Maximum Depth of Binary Tree | Easy | [#104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| Invert Binary Tree | Easy | [#226](https://leetcode.com/problems/invert-binary-tree/) |
| Level Order Traversal | Medium | [#102](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| Binary Tree Right Side View | Medium | [#199](https://leetcode.com/problems/binary-tree-right-side-view/) |

**学ぶパターン:**
```python
# DFS（再帰）の型
def dfs(node):
    if not node:
        return base_case
    left = dfs(node.left)
    right = dfs(node.right)
    return combine(left, right)

# BFS（キュー）の型
from collections import deque
queue = deque([root])
while queue:
    node = queue.popleft()
    for child in [node.left, node.right]:
        if child:
            queue.append(child)
```

---

### Day 5（6/29 日）| グラフ — BFS & DFS

| 問題 | 難易度 | リンク |
|---|---|---|
| Number of Islands | Medium | [#200](https://leetcode.com/problems/number-of-islands/) |
| Clone Graph | Medium | [#133](https://leetcode.com/problems/clone-graph/) |
| Course Schedule | Medium | [#207](https://leetcode.com/problems/course-schedule/) |

**学ぶパターン:**
```python
# グラフDFS（訪問済み管理）の型
visited = set()
def dfs(node):
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs(neighbor)
```

---

### Day 6（6/30 月）| 二分探索

| 問題 | 難易度 | リンク |
|---|---|---|
| Binary Search | Easy | [#704](https://leetcode.com/problems/binary-search/) |
| Search a 2D Matrix | Medium | [#74](https://leetcode.com/problems/search-a-2d-matrix/) |
| Find Minimum in Rotated Sorted Array | Medium | [#153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |

**学ぶパターン:**
```python
# 二分探索の型（境界を間違えない）
left, right = 0, len(nums) - 1
while left <= right:
    mid = (left + right) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

---

### Day 7（7/1 火）| Week 1 復習日

- Day 1〜6で解けなかった問題をもう一度タイムアタック（各30分上限）
- 解けた問題の計算量を口頭で説明する練習
- 「パターンカード」を自分で作る（各パターンのコードテンプレートを手書きで写す）

---

## Week 2（7/2〜7/8）: 応用 & 定着期

**目標:** Medium問題をヒントなしで30〜40分以内に解けるようにする。

### Day 8（7/2 水）| スタック & キュー

| 問題 | 難易度 | リンク |
|---|---|---|
| Valid Parentheses | Easy | [#20](https://leetcode.com/problems/valid-parentheses/) |
| Min Stack | Medium | [#155](https://leetcode.com/problems/min-stack/) |
| Daily Temperatures | Medium | [#739](https://leetcode.com/problems/daily-temperatures/) |
| Largest Rectangle in Histogram | Hard | [#84](https://leetcode.com/problems/largest-rectangle-in-histogram/)（解説のみ） |

---

### Day 9（7/3 木）| ヒープ / 優先度キュー

| 問題 | 難易度 | リンク |
|---|---|---|
| Kth Largest Element in a Stream | Easy | [#703](https://leetcode.com/problems/kth-largest-element-in-a-stream/) |
| K Closest Points to Origin | Medium | [#973](https://leetcode.com/problems/k-closest-points-to-origin/) |
| Find Median from Data Stream | Hard | [#295](https://leetcode.com/problems/find-median-from-data-stream/)（解説のみ） |

```python
# Python ヒープの使い方
import heapq
heap = []
heapq.heappush(heap, val)      # 追加
heapq.heappop(heap)            # 最小値を取り出す
# 最大ヒープは値を負にして使う: heappush(heap, -val)
```

---

### Day 10（7/4 金）| 動的計画法 I（1次元DP）

| 問題 | 難易度 | リンク |
|---|---|---|
| Climbing Stairs | Easy | [#70](https://leetcode.com/problems/climbing-stairs/) |
| House Robber | Medium | [#198](https://leetcode.com/problems/house-robber/) |
| Coin Change | Medium | [#322](https://leetcode.com/problems/coin-change/) |
| Word Break | Medium | [#139](https://leetcode.com/problems/word-break/) |

**DPの考え方:**
```
1. 状態定義: dp[i] = ?（何を表すか）
2. 遷移式: dp[i] = f(dp[i-1], dp[i-2], ...)
3. ベースケース: dp[0], dp[1] を設定
4. 計算順序: 小さい問題から大きい問題へ
```

---

### Day 11（7/5 土）| 動的計画法 II（2次元DP）

| 問題 | 難易度 | リンク |
|---|---|---|
| Unique Paths | Medium | [#62](https://leetcode.com/problems/unique-paths/) |
| Longest Common Subsequence | Medium | [#1143](https://leetcode.com/problems/longest-common-subsequence/) |
| Edit Distance | Hard | [#72](https://leetcode.com/problems/edit-distance/)（解説のみ） |

---

### Day 12（7/6 日）| バックトラッキング

| 問題 | 難易度 | リンク |
|---|---|---|
| Subsets | Medium | [#78](https://leetcode.com/problems/subsets/) |
| Combinations | Medium | [#77](https://leetcode.com/problems/combinations/) |
| Permutations | Medium | [#46](https://leetcode.com/problems/permutations/) |
| Word Search | Medium | [#79](https://leetcode.com/problems/word-search/) |

```python
# バックトラッキングの型
def backtrack(start, current):
    if base_case:
        result.append(current[:])
        return
    for i in range(start, len(candidates)):
        current.append(candidates[i])
        backtrack(i + 1, current)
        current.pop()  # ← これがバックトラック
```

---

### Day 13（7/7 月）| リンクリスト

| 問題 | 難易度 | リンク |
|---|---|---|
| Reverse Linked List | Easy | [#206](https://leetcode.com/problems/reverse-linked-list/) |
| Linked List Cycle | Easy | [#141](https://leetcode.com/problems/linked-list-cycle/) |
| Merge Two Sorted Lists | Easy | [#21](https://leetcode.com/problems/merge-two-sorted-lists/) |
| Reorder List | Medium | [#143](https://leetcode.com/problems/reorder-list/) |

---

### Day 14（7/8 火）| Google頻出問題セット

LeetCode の [Googleタグ問題](https://leetcode.com/company/google/) から Medium を中心に：

| 問題 | 難易度 | リンク |
|---|---|---|
| Meeting Rooms II | Medium | [#253](https://leetcode.com/problems/meeting-rooms-ii/) |
| Walls and Gates | Medium | [#286](https://leetcode.com/problems/walls-and-gates/) |
| Decode Ways | Medium | [#91](https://leetcode.com/problems/decode-ways/) |
| Longest Palindromic Substring | Medium | [#5](https://leetcode.com/problems/longest-palindromic-substring/) |

---

## Week 3（7/9〜7/15）: 面接シミュレーション期

**目標:** 本番と同じ条件で問題を解き、「話しながら解く」を体に染み込ませる。

### Week 3 のルール（必須）

```
✅ タイマー 40分 をセットして解く（延長なし）
✅ 解く前に必ず口頭で方針を話す（録音推奨）
✅ コードを書きながら声で説明する
✅ 書き終わったらエッジケースを自分から3つ挙げる
✅ 最後に時間・空間計算量を述べる
❌ 解説を先に見ない
❌ 40分で解けなくても自己嫌悪しない（ペース感覚をつかむ段階）
```

---

### Day 15（7/9 水）| モック面接 ① — 配列・文字列

自分を面接官だと思って以下を「口頭で解説しながら」解く：

| 問題 | 難易度 |
|---|---|
| Container With Most Water | Medium |
| Trapping Rain Water | Hard |
| Encode and Decode Strings | Medium |

---

### Day 16（7/10 木）| モック面接 ② — 木・グラフ

| 問題 | 難易度 |
|---|---|
| Validate Binary Search Tree | Medium |
| Lowest Common Ancestor of BST | Medium |
| Pacific Atlantic Water Flow | Medium |
| Word Ladder | Hard（解説のみ） |

---

### Day 17（7/11 金）| モック面接 ③ — DP・バックトラッキング

| 問題 | 難易度 |
|---|---|
| Jump Game | Medium |
| Partition Equal Subset Sum | Medium |
| N-Queens | Hard |
| Palindrome Partitioning | Medium |

---

### Day 18（7/12 土）| 弱点集中補強日

Week 1〜2 で苦手だったパターンを特定して集中練習。
以下の観点で自己評価：

```
[ ] Two Pointer   — 30分以内に解けるか？
[ ] BFS/DFS      — パターンを見た瞬間に型が浮かぶか？
[ ] DP           — 状態定義を自分で設計できるか？
[ ] ヒープ        — heapq の API を覚えているか？
[ ] バックトラック  — 再帰の構造を説明できるか？
```

---

### Day 19（7/13 日）| Google想定問題 最終セット

実際のGoogle面接でレポートされた問題（Glassdoor / LeetCode Discuss より）：

| 問題 | 難易度 | パターン |
|---|---|---|
| Next Permutation | Medium | 配列操作 |
| Search in Rotated Sorted Array | Medium | 二分探索 |
| Minimum Path Sum | Medium | 2次元DP |
| Number of Connected Components | Medium | グラフ Union-Find |
| Serialize and Deserialize Binary Tree | Hard | 木 BFS/DFS |

---

### Day 20（7/14 月）| 面接前日 — 総仕上げ

**やること:**
1. 全パターンのコードテンプレートを紙に書いて確認（暗記チェック）
2. 今まで解いた問題から5問ランダムに選び、口頭で「方針だけ」を30秒で説明する練習
3. 計算量の早見表を見直す（下記参照）
4. 早く寝る（これが一番大事）

**やらないこと:**
- 新しい問題を初見で解く
- 難しいHard問題に挑む
- 夜遅くまでコードを書く

---

### Day 21（7/15 火）| 面接前日の最終確認

**当日持っていくメンタルセット:**
- 「面接官はヒントを出すために存在する。沈黙より対話」
- 「完璧な解法を出す必要はない。思考プロセスを見せれば良い」
- 「40分でMediumを1問ちゃんと解ければ十分」

---

## 計算量 早見表

| アルゴリズム / 構造 | 時間計算量 | 空間計算量 |
|---|---|---|
| 配列アクセス | O(1) | — |
| 配列探索（線形） | O(n) | O(1) |
| 二分探索 | O(log n) | O(1) |
| ハッシュマップ 挿入/検索 | O(1) avg | O(n) |
| ソート | O(n log n) | O(log n) |
| BFS / DFS（グラフ） | O(V + E) | O(V) |
| BFS / DFS（木） | O(n) | O(h) ※h=高さ |
| ヒープ 挿入/削除 | O(log n) | O(n) |
| DP（1次元） | O(n) | O(n) or O(1) |
| DP（2次元） | O(n×m) | O(n×m) or O(m) |
| バックトラッキング | O(2ⁿ) or O(n!) | O(n) |

---

## 推奨ツール・リソース

| ツール | 用途 | URL |
|---|---|---|
| LeetCode | 問題練習（Googleタグでフィルタ） | https://leetcode.com/company/google/ |
| NeetCode 150 | 動画解説付き厳選150問 | https://neetcode.io/practice |
| NeetCode YouTube | パターン別解説動画（英語） | https://www.youtube.com/@NeetCode |
| Python Tutor | コードの実行を視覚化 | https://pythontutor.com/ |

---

## 週次チェックリスト

### Week 1 終了時
- [ ] 5パターンの「型」をコードなしで口頭説明できる
- [ ] Easy問題を30分以内に解ける
- [ ] 解いた後に計算量を必ず述べる習慣がついた

### Week 2 終了時
- [ ] Medium問題の50%以上を40分以内に自力で解ける
- [ ] 「解けなくても方針は説明できる」状態になっている
- [ ] エッジケースを自発的に3つ以上挙げられる

### Week 3 終了時（面接前日）
- [ ] 口頭で考えを言語化しながら解ける（沈黙が5秒以上続かない）
- [ ] ブルートフォース → 最適化の流れを自然に説明できる
- [ ] 計算量を「自分から」述べられる
- [ ] 「面接官と一緒に問題を解く」マインドセットができている
