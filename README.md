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
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Mon Jun 7 23:34:13 2021 +0900

    Update README.md

    add explanation about git log --oneline

commit ee93b29c2467f72deeeca2b1de7c95bb19cec052 (origin/master, origin/HEAD)
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Mon Jun 7 23:29:25 2021 +0900

    Update README.md

    add explanation about git log

commit 543e0ad0c95d3efc65276c03fbb8f3b4b4d3f2c4
Author: Yutaka Kobayashi <48763691+kobayashiyutaka@users.noreply.github.com>
Date:   Mon Jun 7 23:23:27 2021 +0900

    Initial commit

```

ここで、これまでにこのリポジトリになされたコミットが表示されており、一番上が最新のコミットになります。

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

