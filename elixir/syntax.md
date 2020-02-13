## 文法基礎

- 這個頁面的目標
  - 了解語法的學習資料
  - 能夠使用 `iex` 來試 Elixir 的語法
  - 知道基本值的種類，並能夠讀懂與使用
- 所需時間: 15 分鐘（包含自由嘗試 `iex` 的時間）

---

### 學習資料

- 語法請使用以下教科書
  - [Official Introduction](https://elixir-lang.org/getting-started/introduction.html)
    - 這份指南會做大概的說明
  - [Elixir School](https://elixirschool.com/zh-hant/lessons/basics/basics/) (中文版對應)
  - Elixir 標準函式庫的 [API References](https://hexdocs.pm/elixir/api-reference.html)
  - 書籍的話，[『プログラミング Elixir』](https://www.amazon.co.jp/dp/B01KFCXP04) 為標準說明書
    - 連結為日文翻譯版，以 Elixir 1.4 為基準。雖然有點舊了，不過與主要版本的語法幾乎相同
    - 公司內也有幾本。需要的時候就去跟上級說吧（有圖書費用，大多可以立刻購買）
- 本文件會特別指定需要了解的部分
- 後端練習中會有會有足夠的時間編寫大量的 Elixir 程式碼，
  所以 **現階段不用將語法完全了解也 OK**
- 反而是對工具鏈的使用方法與整體的 Workflow 的了解比較重要

---

### REPL: `iex`

- 首先將 Elixir 的 REPL - `iex` 開起來吧。打開 Terminal 並輸入 `iex`

```
$ iex
Erlang/OTP 20 [erts-9.3.3.3] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:10] [hipe] [kernel-poll:false]

Interactive Elixir (1.7.4) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

- **[確認]** 如果沒辦法，就表示安裝沒有完全。會花費一點時間所以確認一下
- REPL 為 "Read-Eval-Print-Loop" 的簡稱，重複地進行將每次輸入的程式碼透過編譯器與 VM 做評價計算，並返回結果
  - 很多其他語言也有
- 基本上大多會用來做語法的測試或動作確認等
  - 有些語言會以 REPL 為中心進行開發
- 最後還是會正式地介紹建立 Application 專案的方式，不過先用 `iex` 來試試語法吧
- 要將 REPL 結束的話，按**2 次** `Ctrl + c`

---

### 基本值

資料: [Basic types - Elixir](https://elixir-lang.org/getting-started/basic-types.html)

- 基本值的種類（型別）如下

```elixir
iex> 1          # integer
iex> 0x1F       # integer
iex> 1.0        # float
iex> true       # boolean
iex> :atom      # atom / symbol
iex> "elixir"   # string
iex> [1, 2, 3]  # list
iex> {1, 2, 3}  # tuple
```

- 請在 `iex` 輸入看看
- 也試試看語法輸入錯誤的情況下 `iex` 會如何反應
- 以下說明其他語言可能沒有的部分

---

- **Atom; 原子** : 以 `:` 為前置符號的值，有實體在 VM 只存在一個這樣的特點
- 通常用於表示在編譯期（編碼時）的固定常數
- 由於實體只有一個，就算在程式內多處重複使用同樣的 Atom ，其值不會被複製，只會增加對現存 Atom 的參考
- 一旦被產生的 Atom 在 VM 裡面不會消失（不會被 Garbage Collect）
  - 因此不應該動態地產生 Atom。動態的值請使用 **String; 字串**

---

- **Tuple; 元組** 是多個值所組成的值
- 組成值的類型（型別）可以任意。組合的數量也不限
- 但是與 **List; 列表** 不同，「組合方式」也就是 tuple 的長度是在編譯期（編碼時）決定。
  換句話說，開發者用來表達某種「有意義的組合」的時候使用
  - 例如將 2 個 float 組合來表示「座標」
- 相反地，如果一個類型（型別）的值存在 0 個以上，其數量會在執行時增減的情況，請使用 List

---
- 屬性（大小或內容）是在編譯期（編碼時）決定的，還是在執行的時候變動的，
  注意這一點在任何語言都有了解的必要，請記住吧

---

### 函式（匿名函式）

資料: [Anonymous Functions](https://elixir-lang.org/getting-started/basic-types.html#anonymous-functions)

- 函式可以分為，在哪都能寫的匿名函式; anonymous function 與必須屬於 模組; module 的具名函式; named function
- 在 REPL 可以馬上試試匿名函式

```elixir
iex> add = fn a, b -> a + b end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> add.(1, 2)
3
```

- `fn` ~ `->` ~ `end` 這樣的語法
  - 引數的部分有沒有用 `()` 包起來都沒關係。也就是說可以寫成 `fn(a, b) -> a + b end`
  - 回傳了 `#Function<12.71889879/2 in :erl_eval.expr/5>`，這表示被給予辨別用的 Hash 值的匿名函式
  - `/2` 是在表示 Erlang/Elixir 的函式時所使用的記法。`/` 的後面會附上引數的數量
- 可以綁定在變數上，並用來呼叫
  - （小測驗）也可以不綁定在變數上就能夠呼叫。該怎麼寫呢？
- 呼叫匿名函式是用 `.()` 這樣的語法
  - 這是用來避免在 Elixir 中可以省略 `()` 這樣的語法功能所產生的曖昧性
- 也有省略引數名的寫法

```elixir
iex> fun = &(&1 + 1)
#Function<6.71889879/1 in :erl_eval.expr/5>
iex> fun.(1)
2
```

- 用 `&()` 包起來，引數依序以 `&1`、`&2`...來表示。 之後會開始有高階函式出現，那時就會使用這樣的省略記法

---

- 各種嘗試看看吧
- 之後會再介紹到關於具名函式與模組

---

### 運算子; Operators

資料: [Basic operators - Elixir](https://elixir-lang.org/getting-started/basic-operators.html)

- 上面的例子已經出現過像 `+` 這種運算子（中綴運算子; infix operators）
- 連接 List

```elixir
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
```

- 連接字串

```elixir
iex> "foo" <> "bar"
"foobar"
```

- 中綴運算子實際上也是函式，在文件內可以參考 [`++/2`](https://hexdocs.pm/elixir/Kernel.html#++/2)
