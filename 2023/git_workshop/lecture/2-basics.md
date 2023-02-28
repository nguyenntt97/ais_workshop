# Gitの基本コマンド

このセクションでは、Gitの基本的なコマンドを紹介します。これらのコマンドは、作業環境において日常的に使用する最も一般的なものです。

`Git`のコマンドはたくさんあり（約`140コマンド`）、高度な概念もあるため、このワークショップの目的は、誰もが研究において基本的に必要なGitを使えるようになるための導入にとどまるものです。

このノートに目標は以下の通り、

- [ ] gitの基本的なコマンドを理解し、使用することができる (`clone`,`init`,`fetch`,`config`,`pull`,`add`,`commit`,`branch`,`checkout`,`diff`)
- [ ] 基本的なコマンドを適用して目的を達成することができる
- [ ] 自分のワークスペースのバージョン管理で一般的なシナリオを理解することができる

## Setup Git

### Gitのインストールを確認

```bash
$ git --version
git version 2.30.2.windows.1
```

### 設定ID

多くのユーザーが同じリポジトリで作業する可能性があるため、すべてのコミットには作者名（ユーザー名とメールアドレス）を含める必要があります。
そのため、将来のコミットに含めるために、自分の情報を git に宣言する必要があります。

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
<p align="center">
<img src="https://cdn-ak.f.st-hatena.com/images/fotolife/i/itstaffing/20200617/20200617134342.jpg" height="150" />
<br>
<em>Fig. 他のチームメンバーがリビジョンの原因を知り、連絡を取るためのユーザー名と電子メール. <br>画像作：あいさん・r-staffing</em>
</p>

### 現在設定を確認

現在のすべての設定を確認するには

```bash
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

詳細については—see [[SCM Book]](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

--- 

## Scenario 1

### シナリオの説明

新しいプロジェクトのためにローカルリポジトリを作成したいとします。後は、変更を加えたので、変更内容を確認したい。

対象コマンド: `init`, `add`, `commit`, `log`

### 1. 新しいgitフォルダの作成

```bash
$ mkdir manga_db
$ cd manga_db
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

### 2. リポジトリ内のファイルの状態を確認する

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

| Gitのリポジトリフォルダ内のファイルは、2つの状態のいずれかになります:
| - Tracked - Gitが知っていて、リポジトリに追加されたファイル
| - Untracked - 作業ディレクトリにあるが、リポジトリに追加されていないファイル

### Repository

リポジトリとは、あるものの倉庫を意味する。

Gitでは、Gitにバージョン管理をさせたいディレクトリにリポジトリを作成します。一台のマシンや一人のユーザーが多くのリポジトリを持つことができます。

マシンにGitソフトウェア（またはGitクライアント）をインストールした後：

- 自分用のリポジトリ（ローカルリポジトリ, `Local Repository`）を作成する
- リモートリポジトリ（`Remote Repository`; `Remote` = `ローカルではない`）をクローンする

### git init

### 

- チームリーダーがプロジェクトのチーム・リポジトリ (`Repository` or `Git Repo.`) を作成
- 自分のマシンにGitシステムをインストールする必要があります。
- すべての貢献者は、元のリポジトリ（またはリモートリポジトリ, `Remote Repository`）を自分のマシンにクローン(`clone`)します。貢献者は自分のローカルリポジト(`Local Repository`)リを変更します。

<p align="center">
<img src="https://www.w3docs.com/uploads/media/default/0001/03/3f26b30cc1dbda3424ceef3ab4977149906a0c58.png" height="300" />
<em>`clone` は、自分のマシンにリモートリポジトリのコピーを作成します。</em>
</p>

- 自分のローカルコピーをプロジェクトの最新版(他の貢献者による変更)に更新するには、`git pull`（`pull` = `取って`）を使用します。
- いくつかの変更 (`change`) を行った後、投稿者は変更を意味のあるバージョン・コミット (`commit`) にまとめることができます。
- 多くのコミットを行った後、チームリポジトリに一括でプッシュ (`push`<>`pull`, `push`=`置いて`)することができます。

バージョン管理では、チームメンバー全員がプロジェクトのコピー（ローカルリポジトリ "Local repository"）を持っています。プロジェクトのオリジナルコピー（リモートリポジトリ "Remote Repository"）は、会社のマシンに保管されていました。