## 模式比對與控制語法

- 這個頁面的目標
  - 模式比對的
  - 學習模式比對
  - 接觸且能夠理解常用的控制語法（`case`）
- 所需時間: 20 分鐘

---

### 模式比對的基礎

資料: [Pattern matching - Elixir](https://elixir-lang.org/getting-started/pattern-matching.html)

- Elixir 有強大的**模式比對**語法
  - 雖然功能有差，但許多語言都有類似的功能
  - 雖然與程序式語言的「變數」與「指派」類似，但思考方式有點不同，且更有彈性
- 如果從其他語言轉過來的話可能會有些混亂，在此先做個說明

---

- 例如以下的 List

```elixir
iex> l = [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
```

- 雖然出現了 `=`，但在 Elixir 中 `=` 實際上是**比對運算子**，並非「指派」運算子
- 這邊首先在左邊的 `l` 變數並不存在任何條件（也就是自由變數），
  因此會將右邊的 List 以整個都「接受」的方式做「比對」，
  - 自由變數是像是可以成為任何型別的容器，根據目標值而改變型別並**保持**
- 結果 `l` 會被指派成右邊的 List
  - 也就是說右邊的 List 被**綁定; bind**到 `l` 了。變成有意義（值），而不再「自由」了
- 試試以下來證明看看

```elixir
iex> [1, 2, 3, 4, 5] = m
** (CompileError) iex:1: undefined function m/0
```

- 雖然 `m` 是新的變數，但並沒有在任何地方被定義。Elixir 將這個認為是 `m/0` 這樣的函式，不過當然會找不會於是發生錯誤
- 另一方面，

```elixir
iex> [1, 2, 3, 4, 5] = l
[1, 2, 3, 4, 5]
```

- **如果是 `l` 的話這個式子就成立！**
- 因為 `l` 已經被綁定了 List，且與左邊的內容完全相同
- 於是我們知道 Elixir 的 `=` 變成不是「指派」而只是「比對運算子」了

---

- 比對運算子的左邊可以表示各種「條件」
  - 已經有「完全無條件」（產生新變數）與「（像某個 List 的）固定值」這兩個模式出現了
- 不過也有像是「不是空的 List」這樣的條件

```elixir
iex> [head | tail] = [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
iex> head
1
iex> tail
[2, 3, 4, 5]
```

- `[head | tail]` 這個語法會與不是空的 List 比對，
  - `head` 為頭端的值
  - `tail` 為去掉頭的 List
- 並分別綁定到這兩個變數上
  - 其實，能夠像這樣以「頭端與其之後」的形式做比對是因為 Elixir 的 List 是 [Linked List](https://elixir-lang.org/getting-started/basic-types.html#linked-lists)。
    所謂 Linked List 是以「值與其下一個值的參考值」的資料鍊來表示的 List 的一種，對頭端值的存取（讀出、新增）快速的資料結構
  - 在函數式語言比較多會使用 Listed List 作為主要的資料結構而非陣列; Array
- 若給予空的 List 的話，

```elixir
iex> [head | tail] = []
** (MatchError) no match of right hand side value: []
```

- 會發生錯誤
- 其他還有很多模式比對能夠使用的條件與綁定的寫法，請透過實際寫程式來逐漸習慣
  - 特別是使用 pin operator 的模式比對，雖然有點難不過偶爾會使用到，所以之後再學一下吧。資料連結有詳細說明

---

### `case`

資料: [case, cond, and if - Elixir](https://elixir-lang.org/getting-started/case-cond-and-if.html)

- 模式比對除了在比對運算子 `=` 之外還可以使用在
  - `case`
  - 函式的引數
- 例如像以下的模式比對很常見
  - 要將下面的例子在 `iex` 直接執行需要花費一些力氣，所以可以看看就好

```elixir
case some_list do
    [] ->
        "empty!"
    [head | _] ->
        "first value: " <> to_string(head)
end
```

```elixir
fn
    [] ->
        "empty!"
    [head | _] ->
        "first value: " <> to_string(head)
end
```

- 在模式比對中，若不想將比對到的值綁定到變數的情況可以使用以 `_` 作為前置名稱的變數
- 如你所見，在 `case` 中的模式比對 **基本上就是條件分歧**
  - 雖然在 Elixir 以**布林值; boolean**為條件的情況下會使用 `if`，
    但如果是對其他類型的值做條件分歧的話，`case` 是最常用的
- （在例子中出現的 `to_string/1` 是標準函式庫所提供的字串轉換函式）
