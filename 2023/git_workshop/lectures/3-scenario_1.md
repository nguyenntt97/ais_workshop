# Scenario 1

新しいプロジェクトのためにローカルリポジトリを作成したいとします。後は、変更を加えたので、変更内容を確認したい。

対象コマンド: `init`, `add`, `commit`, `log`

1. [新しいgitフォルダの作成](#1-新しいgitフォルダの作成)
2. [リポジトリ内のファイルの状態を確認する](#2-リポジトリ内のファイルの状態を確認する)
3. [変更をコミットにまとめる](#3-変更をコミットにまとめる)
4. [実践編1: git commit](#4-実践編1-git-commit)

## 1. 新しいgitフォルダの作成

```bash
mkdir manga_db
cd manga_db
```

```bash
$ git init
Initialized empty Git repository in /mnt/d/projects/manga_db/.git/
```

初めてローカルの Git リポジトリを作成したところです。しかし、それは空っぽです。

そこで、いくつかのファイルを追加するか、お気に入りのテキストエディタで新しいファイルを作りましょう。そして、先ほど作成したフォルダに保存するか移動させましょう。

最初に `naruto.txt` ファイルを作成します。

```plain
# ./naruto.txt

naruto
sasuke
zoro
sakura
```

そして、次のような内容の `onepiece.txt` ファイルを作成します。

```plain
# ./onepiece.txt

luffy
nami
boa
```

2つのファイルを作成した後、確認してください。

```bash
$ pwd
/mnt/d/projects/manga_db

$ ll
total 0
-rwxrwxrwx 1 elchris elchris 42 Feb 28 23:15 naruto.txt
-rwxrwxrwx 1 elchris elchris 36 Feb 28 23:17 onepiece.txt
```

## 2. リポジトリ内のファイルの状態を確認する

次に、Git の状態をチェックして、それが私たちのレポの一部であるかどうかを確認します。

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        naruto.txt
        onepiece.txt

nothing added to commit but untracked files present (use "git add" to track)
```

`git status` は、リポジトリの現在の状態を表示します:

- どの git ブランチ: `On branch **master**`
- 未投稿のコミット一覧: `No commits yet`
- 未トラックのファイル: `Untracked files (naruto.txt, onepiece.txt)`

新しく追加された２つのファイル (i.e., `naruto.txt`, `onepiece.txt`) は、Gitによって認識されましたが、追跡されませんでした。

Gitは追跡しているファイルのバージョン変更のみを監視します。そのため、ユーザーはGitが追跡する必要のあるファイルを宣言しなければなりません。

> Gitのリポジトリフォルダ内のファイルは、2つの状態のいずれかになります:
>
> - Tracked - Gitが知っていて、リポジトリに追加されたファイル
> - Untracked - 作業ディレクトリにあるが、リポジトリに追加されていないファイル

Gitでファイルを追跡するには、次のようにします。

```bash
$ git add naruto.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   naruto.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        onepiece.txt

$ git add onepiece.txt
...
```

また、すべてのファイルを同時に追加することもできます。

```bash
$ git add .
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   naruto.txt
        new file:   onepiece.txt
```

> Git フォルダ内の任意の場所に、単一のファイルあるいはディレクトリ全体を追加することができます。

> `.` はカレントディレクトリを表します。
>
> `git add .` は、カレントディレクトリとその子から、追跡されていないファイルをすべて追加することを意味します。

> もし、ルートディレクトリにいないときにすべてのファイルを追加したい場合は、`git add --all` を使用してください。

## 3. 変更をコミットにまとめる

```bash
$ git commit -m "my first commit"
[master (root-commit) 473f896] my first commit
 2 files changed, 11 insertions(+)
 create mode 100644 naruto.txt
 create mode 100644 onepiece.txt
```

Gitがコミットを追跡しているかどうかを確認するには

```bash
$ git log
commit 473f896d358535b5473205c1661a9d40b99382e4 (HEAD -> master)
Author: elchris <nguyenng178@gmail.com>
Date:   Tue Feb 28 23:49:27 2023 +0900

    my first commit
```

すべてのコミットに対しては

- Gitに割り当てられた一意のハッシュで: `...99382e4`
- コミットのブランチ: `(HEAD -> master)`
- 著者名
- コミットメッセージ

## 4. 実践編1: Git Commit

### 実践編1: 要求事項

1. リポジトリに新しいファイルを追加しました (`toloveru.txt`)。内容については

```plain
# ./toloveru.txt

riku
lala
haruna
```

2. `zoro` の行を `naruto.txt` から `onepiece.txt` に移動する。
3. これら2つの変更をメッセージ付きのコミットにまとめます。(例: `-m "トローベルを追加と間違った世界を修正するゾロ"`)
4. 自分のコミットが Git に記録されているかどうかを確認するには、`git log` を使用します。

*すべての要件が終了したら、次の方法で正しく実行されているかどうかを確認することについては*ーsee [[完成度検証]](#実践編1-完成度検証)

---

### 実践編1: 完成度検証

必要事項を確認するには、以下の手順に従ってください。

1. リポジトリに3つのファイルがあることを確認します。

```bash
$ ll
total 0
-rwxrwxrwx 1 elchris elchris 42 Feb 28 23:58 naruto.txt
-rwxrwxrwx 1 elchris elchris 36 Feb 28 23:17 onepiece.txt
-rwxrwxrwx 1 elchris elchris 35 Mar  1 09:52 toloveru.txt
```

2. 次に、ゾロはNARUTOではなくONE PIECEにいるべきということで、`naruto.txt`から削除し、`onepiece.txt`に追加することにします。

```bash
$ git status --short
 M naruto.txt
 M onepiece.txt
?? toloveru.txt
```

3.

```bash
$ git log                                                
commit 08849046e901bf9e68476cedb9525abe10bab2dd (HEAD -> master)
Author: ***** <*****>
Date:   *****

    トローベルを追加と間違った世界を修正するゾロ

commit cda059ec679ac7752dc695bb4ac1d92aad3f0472
Author: ***** <*****>
Date:   *****

    my first commit
```

---
[[MAIN]](../README.md) - [[BACK: Ch.2 Gitの基本コマンド]](./2-basics.md)