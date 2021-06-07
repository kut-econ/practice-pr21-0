# Gitの練習

## このファイルについて

これはKUT経・マネの講義「プログラミング」においてGitの使い方を練習するためのリポジトリです。

## git logの使い方

次のコマンドにより、レポジトリの履歴を閲覧することができます。

```bash
git log
```

結果は次のようになります。

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

過去のコミットを無かったことにするのは`reset`と言います。`reset`には3種類あります。

|resetのオプション|機能|
|--|--|
|`--hard`|作業ディレクトリもステージングエリアも両方戻す|
|`--soft`|履歴だけ戻す|
|`--mixed`|履歴とステージングエリアだけ戻す|

なおオプション無しのリセットは少し意味が違い、ステージングの取り消しを意味します。引数には、どこまでもとに戻したいかを指定します。

たとえば、最新のコミットだけ取り消したい場合は、

```bash
git reset --soft HEAD^
```

とします。最新と一つまえのコミットを取り消す場合は、

```bash
git reset --soft HEAD^^
```

とします。`--soft`オプションでは、変更はステージングするまえの状態になります。ステージングエリアももとに戻してしまいたい、`--mixed`にします。作業ディレクトリもステージングエリアも元に戻す、つまり何もかもなかったことにするには、`--hard`を指定します。

