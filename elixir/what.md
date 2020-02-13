## 什麼是 Elixir

- 這個頁面的目標
  - 認識關鍵字、掌握 Elixir 大概的特徵
- 所需時間: 5 分鐘

- 官方網站 => https://elixir-lang.org/

> Elixir is a dynamic, functional language designed for building scalable and maintainable applications.

- 動態型別
- 函數式
- 能寫出 [Scalable][dist] 的 Application
- 能寫出 [Maintainable](https://skirino.github.io/slides/advice_for_new_graduates.html) 的 Application
- 作者為 [José Valim](https://github.com/josevalim)。主要由 [Plataformatec 公司](http://plataformatec.com.br/) 支援開發

[dist]: ../basics/distribution.html

> Elixir leverages the Erlang VM, known for running low-latency, distributed and fault-tolerant systems,
> while also being successfully used in web development and the embedded software domain.

- 使用 [Erlang VM](http://www.erlang.org/)
- 能夠低延遲地執行，
- [分散的][dist]、
- 且高容錯之系統的 VM
- 由 Ericsson 公司開發，自 1998 年 OSS 化

---

### 與 Erlang 的 **互用性; interoperability**

- Elixir 的程式碼會被編譯成 Erlang VM(BEAM) 用的 bytecode(.beam 檔)，並由 Erlang VM 執行
- 編譯後的 beam 檔與從 Erlang 編譯過的相等，同時也可以組合起來執行
- 在 Erlang VM 中的程式碼管理單元為 **module**，不過也可以在 Elixir 呼叫 Erlang 的 module
- 其他也有類似特性的語言或 VM
  - 例）Scala 的程式碼會被編譯成 JVM 能夠執行的 bytecode，可以與 Java 程式組合執行

---

### 其他語言特徵

- 語法看起來與 Ruby 相似，但**只是相似而已**
- 強大的**Pattern Matching**
- 因為直接使用 Erlang VM，因此繼承 Erlang VM 的特點
  - 輕量 Process
  - Message Passing
  - OTP（輕量 Process 的階層結構、局部化崩潰、自動復原機制）
  - 分散式計算
- 在編譯期的程式碼自動生成等，能夠進行各種處理的環境（Macro、Metaprogramming）

---

### 工具鍊

- 語言綁定工具
  - `mix` - 建構工具兼 Task 執行
  - `iex` - REPL
  - `ex_unit` - Testing Library
- [hex.pm](https://hex.pm) - Package 管理
- `ex_doc` - 文件生成工具
  - [hexdocs.pm](https://hexdocs.pm) - 線上文件
