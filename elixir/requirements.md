## 環境建立

- 確認環境已建立完成。所需時間: 5 分鐘內
- 如果沒有準備的話要多花點時間。～20 分鐘

---

- **請在 Hands on 之前將完成以下準備**
- 公司內推薦使用 [asdf](https://github.com/asdf-vm/asdf)
  - asdf 的詳細請參考[官方指南](https://asdf-vm.com/#/core-manage-asdf-vm)
- 先將 Erlang 20.3.8.9 與 Elixir 1.7.4 裝好吧 (2019/05)

### 以防萬一：大致的步驟

- (假設使用 Bash)

```
$ git clone https://github.com/asdf-vm/asdf.git ~/.asdf
$ source ~/.asdf/asdf.sh
$ brew coreutils automake autoconf openssl libyaml readline libxslt libtool unixodbc wxmac unzip curl
$ asdf plugin-add erlang
$ asdf plugin-add elixir
$ asdf install erlang x.y.z && asdf global erlang x.y.z
（這邊會花費一些時間）
$ asdf install elixir i.j.k && asdf global elixir i.j.k
```

- openssl 是必須的，容易忘記請注意！

  - 沒有 openssl 就直接裝 Erlang 之後發生與 `ssl` 相關的錯誤的話，請將 Erlang 重新安裝

    ```
    $ brew coreutils automake autoconf openssl libyaml readline libxslt libtool unixodbc wxmac unzip curl
    $ asdf uninstall erlang x.y.z
    $ asdf install erlang x.y.z
    ```

## Terminal、編輯器

- Terminal 剛開始使用 macOS 標準的 Terminal.app 即可
  - 個人推薦能夠彈性地對視窗做分割的[iTerm2](https://www.iterm2.com/)
- 先準備好文字編輯器吧！什麼都可以
  - 剛開始有個 Elixir 的語法高亮功能可能會比較好
