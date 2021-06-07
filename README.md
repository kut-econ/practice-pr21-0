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

```

ここで、これまでにこのリポジトリになされたコミットが表示されており、一番上が最新のコミットになります。`commit`の右隣に表示されている長い16進数の数字は、SHA1-ID(シャーワンID)といって、コミットのハッシュ値です(ハッシュに関しては、後の講義で取り上げます。)SHAは[セキュア・ハッシュ・アルゴリズム](https://ja.wikipedia.org/wiki/SHA-1)の訳です。

各コミットを一行に表示したければ、次のようにします。

```bash
git log --oneline
```

```git
35d6346 (HEAD -> master) Update README.md
ee93b29 (origin/master, origin/HEAD) Update README.md
543e0ad Initial commit
```

この場合、メッセージは、最初の一行だけが表示されます。

