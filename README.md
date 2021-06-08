# Gitの練習

- [Gitの練習](#gitの練習)
  - [このファイルについて](#このファイルについて)
  - [git logの使い方](#git-logの使い方)
  - [過去のバージョンを閲覧する](#過去のバージョンを閲覧する)
  - [ブランチを作る](#ブランチを作る)
  - [ブランチをマージする](#ブランチをマージする)
  - [過去のコミットを打ち消す](#過去のコミットを打ち消す)
  - [過去のコミットを無かったことにする](#過去のコミットを無かったことにする)
  - [コマンドエイリアスを作る](#コマンドエイリアスを作る)
  - [.gitignoreファイル](#gitignoreファイル)
  - [ファイル削除のステージング](#ファイル削除のステージング)
  - [ファイル名変更のステージング](#ファイル名変更のステージング)
  - [コミットにタグを付ける](#コミットにタグを付ける)

## このファイルについて

これはKUT経・マネの講義「プログラミング」においてGitの使い方を練習するためのリポジトリです。

## git logの使い方

次のコマンドにより、レポジトリの履歴を閲覧することができます。

```bash
git log
```

結果はたとえば次のようになります。

```git
commit 1c75357290ce957a2a90cf4040ed9d1de15a0459 (HEAD -> master, origin/master, origin/HEAD)
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Tue Jun 8 00:00:08 2021 +0900

    Update README.md

    modify the output of git log

commit a1bf77a96495cc2b67445c4035a48aec93b53f9d
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Mon Jun 7 23:48:49 2021 +0900

    Update README.md

    add the result of git log

commit 35d634694cdc17437b5fb3d73ce69b2483d136c4
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Mon Jun 7 23:34:13 2021 +0900

    Update README.md

    add explanation about git log --oneline

```

ここで、これまでにこのリポジトリになされたコミットが表示されており、一番上が最新のコミットになります。`commit`の右隣に表示されている長い16進数の数字は、SHA1-ID(シャーワンID)といって、コミットのハッシュ値です(ハッシュに関しては、後の講義で取り上げます。)SHAは[セキュア・ハッシュ・アルゴリズム](https://ja.wikipedia.org/wiki/SHA-1)の訳です。

なお、`git log`の出力は、`less`という名前の**ページャー**プログラムに自動的にリダイレクトされています。ちょっとわかりにくいと思いますが、`less`というプログラムはファイルの内容を閲覧するLinuxの標準的なプログラムです。`j`を押すと下に進み、`k`を押すと上に戻ります。`q`でプログラムを終了することができます。

各コミットを一行に表示したければ、次のようにします。

```bash
git log --oneline
```

```git
1c75357 (HEAD -> master, origin/master, origin/HEAD) Update README.md
a1bf77a Update README.md
35d6346 Update README.md
ee93b29 Update README.md
543e0ad Initial commit
```

この場合、メッセージは、最初の一行だけが表示されます。

## 過去のバージョンを閲覧する

過去のバージョンを閲覧したいときは、`checkout`します。

```bash
git checkout 35d6
```

`checkout`の引数には、SHA1-IDの最初の数桁を渡します。

すると、作業ディレクトリの中身がコミット35d6346...時点のものになります。

この状態で`git log master`すると、`HEAD`が`master`とずれているのが分かると思います。`HEAD`があるのが現在閲覧しているコミットで、`master`が最新のコミットです。

最新のコミットまで戻ってくるには、`master`を`checkout`します。

```bash
git checkout master
```

## ブランチを作る

**注)ここからの作業は慎重に!**

それでは、最新のコミットから、別のバージョンを作ってみましょう。別バージョンのことをGitではブランチと言います。ここではbranchの名前はnewにします。

```bash
git branch new
```

ブランチを切り替えるには、`new`を`checkout`します。

```bash
git checkout new
```

現在`new`ブランチにいることを、確かめるには、`git branch`します。

```bash
$ git branch
  master
* new
```

アスタリスクがついているのが現在いるブランチです。

それでは、このバージョンに、新たなファイルを付け足してみましょう。作業ディレクトリの中に`new_code.py`という名前のファイルを作って、適当にPythonコードを書き込んで見てください。

適当にPythonコードを編集しながら、2～3回コミットしてください。

さて、この状態で`git log --oneline`して見てください。

```git
b673206 (HEAD -> new) Update new_code.py
ad627f4 Update README.md slightly
a69eb64 Add new_code.py and update README.me accordingly
1c75357 (origin/master, origin/HEAD, master) Update README.md
a1bf77a Update README.md
35d6346 Update README.md
ee93b29 Update README.md
543e0ad Initial commit
```

以前のブランチ`master`よりも`new`が先に進んでいることが分かると思います。

## ブランチをマージする

新しいブランチの内容に満足して、これをメイン・バージョンにしたいときは、もとのブランチに合流させます。これによって、ブランチ`new`で試していた変更が全て`master`に反映されます。

まずは、本流のブランチ`master`を`checkout`します。

```bash
git checkout master
```

`git branch`で、ちゃんと`master`に戻ったことを確認してください。

```bash
$ git branch
* master
  test
```

次のコマンドで、`new`ブランチを`master`にマージさせます。

```bash
git merge new
```

これで、`new`の変更が全て`master`に反映されました。

ブランチ`new`を`master`にマージしたら、`new`はもう必要ありませんので、次のようにして削除しておいてください。

```bash
git branch -d new
```

## 過去のコミットを打ち消す

Gitでは、過去のコミットを打ち消すコミットをすることができます。これを`revert`といいます。

```bash
git revert ****
```

ここで`****`は打ち消したいコミットのSHA1-IDです。最新のコミットを`revert`したいならば、SHA1-IDの代わりに`HEAD`を指定できます。

```bash
git revert HEAD
```

これを試してみましょう。

先ほど作った`new_code.py`のコードの数行を削除してみてください。そのうえで、変更をコミットしましょう。

変更がコミットできたら、今行ったコミットを`revert`で打ち消してみます。`git log`で、今行ったコミットのIDを調べて、上記のように`revert`してみてください。

`revert`はあくまで打ち消しコミットなので、メッセージの入力を求められますが、デフォルトのままで良いでしょう。

さて、`revert`ができたら、削除した行が復活するはずです。

## 過去のコミットを無かったことにする

過去のコミットを無かったことにしてある状態までもとに戻すことを`reset`と言います。`reset`には3種類あります。

|resetのオプション|機能|
|--|--|
|`--hard`|作業ディレクトリもステージングエリアも両方戻す|
|`--soft`|履歴だけ戻す|
|`--mixed`|履歴とステージングエリアだけ戻す|

なおオプション無しのリセットはステージングの取り消しを意味します。引数には、どこまでもとに戻したいかを指定します。

たとえば、最新のコミットだけ取り消したい場合は、

```bash
git reset --soft HEAD^
```

とします。

![reset-soft](img/soft.drawio.png)

最新と一つまえのコミットを取り消す場合は、

```bash
git reset --soft HEAD^^
```

とします。`revert`が打ち消したいコミットを指定するのに対し、`reset`は取り戻したいコミットを指定することに注意してください。`--soft`オプションでは、変更はステージングするまえの状態になります。ステージングエリアももとに戻してしまいたい、`--mixed`にします。

![reset-mixed](img/mixed.drawio.png)

作業ディレクトリもステージングエリアも元に戻す、つまり何もかもなかったことにするには、`--hard`を指定します。

![reset-hard](img/hard.drawio.png)

それではコミット履歴だけもとに戻すソフトリセットをしてみましょう。

`new_code.py`の行を数行削除してコミットしてください。コミットしたら、`git log`で状態を確認しておいてください。

次に、一つ前のコミットにリセットしてみましょう。

```bash
git reset --soft HEAD^
```

削除した行が戻るはずです。`git log`でコミット履歴の状態を調べておいてください。`master`と`HEAD`が共に一つまえのコミットに戻っているはずです。

次に`git status`をして、ステージングエリアの状況を調べてください。コミットする前の変更がステージされているはずです。これを再コミットしても良いですが、作業エリアもステージングエリアも綺麗に元通りにしておくことにします。

そのためには、現在の`HEAD`の位置にステージングエリアも作業エリアも合わせればよいので、次のようにします。

```bash
git reset --hard HEAD
```

## コマンドエイリアスを作る

`git log`には`--oneline`などの便利なオプションがたくさんあります。たとえば、次のように入力してみてください。

```bash
git log --graph --oneline --all --abbrev-commit
```

各オプションには次のような意味があります。

|オプション|意味|
|--|--|
|`--graph`|ブランチの関係を線で表示|
|`--oneline`|1行表示|
|`--all`|全てのブランチを表示|
|`--abbrev-commit`|コミットIDを短縮表示|

いちいちこういったオプションを打ち込むのは面倒です。そういうときは、コマンドにエイリアスを作ることができます。

ここでは、`git`ユーザーに一般的に使われているエイリアスである`lol`を定義してしまいましょう。`bash`で次のように入力します。

```bash
git config --global alias.lol "log --graph --oneline --all --abbrev-commit"
```

次のようにして`lol`が定義されたかチェックしてください。

```bash
git lol
```

## .gitignoreファイル

通常、作業エリアにおいたファイルは、全てgitのリポジトリにより、「追跡されていない新規ファイル」として認識されてしまいます。従って、`git add -A`すると、必ず追跡されてしまいます。また、一度ステージングすると、必ず

リポジトリの中にあるファイルがリポジトリから無視され、追跡されないようにするには、作業ディレクトリの中に、`.gitignore`というファイルを作成し、その中に、無視するファイル名の一覧を記入します。

たとえば、`.vscode`というディレクトリ、`code`というディレクトリをまるごと無視してほしい、`sample-`で始まる名前のファイルを全て無視してほしい、という場合は、次のように記入します。

```git
.vscode/
code/
sample-*
```

それでは、`local-memo.md`というファイルを作成したうえで、`git status`してください。次のように表示され、リポジトリに検知されていることが分かります。

```git
ブランチ master
Your branch is up to date with 'origin/master'.

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

local_memo.md

nothing added to commit but untracked files present (use "git add" to track)
```

それでは、VS Codeで.gitignoreというファイルを作り、以下のように記述してください。

```git
local-*
```

これで、`local-memo.md`は検知されなくなったはずです(`git status`してみてください)。

しかし、この方法では、`.gitignore`自体は検知されますので、これも無視してほしいときは、

```git
local-*
.gitignore
```

## ファイル削除のステージング

次に、リポジトリからファイルを削除する方法について考えます。まずは、空のファイル`test`を作ってコミットしてください。空のファイルを作るには、`touch`コマンドが使えます。次のようにしてください。

```bash
touch test
```

`git status`して新しいファイルが検知されていることをチェックし、コミットしてリポジトリに追加してください。

次に、リポジトリの中のファイルをリストアップしてみましょう。次のコマンドを使います。

```bash
$ git ls-files
README.md
new_code.py
test
```

表示される一覧に`test`があれば、ファイルがリポジトリに追加されています。なお、作業エリア内のファイルをリストアップするには、単に

```bash
$ ls
README.md  local-memo.md  new_code.py  test
```

と入力します。

作業エリアからファイルを削除しても、リポジトリからは削除されません。リポジトリと作業エリアの両方からファイル`test`を削除するには、次のようにします。

```bash
git rm test
```

これにより、『`test`の削除』という変更がステージングされますので、`git status`して確認してください。最終的に、この変更をコミットすれば、`test`がリポジトリから削除されます。`git ls-files`して確かめてみてください。

## ファイル名変更のステージング

もう一度空のファイル`test`を作ってコミットしてみてください。今度はこのファイルの名前を変更してみます。

作業ディレクトリのファイル名を変更すると、そのファイルは新しいファイルと認識されてしまいます。これを避けるためには、リポジトリ内のファイルと作業エリアのファイルの両方を同時に変更する必要があります。

たとえば、`test`を`test2`に変更する場合は、次のようにします。

```bash
git mv test test2
```

`git ls-files`および`ls`で、作業エリアとリポジトリの両方のファイル名が変更されたことを確認してください。

## コミットにタグを付ける

