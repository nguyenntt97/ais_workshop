# Scenario 3

GitHubのウェブインタフェースでファイルを変更し、ウェブ上でコミットします。複数のローカルレポがリモートレポとやりとりしている場合、コンフリクトが発生する可能性がある:

- 複数のマシン（ラップトップとデスクトップ）があり、それぞれにローカルリポジトリがあります。
- 複数人で同じプロジェクトに取り組む。

このような場合、ローカルコードを`pull`ときと`push`ときで齟齬が生じやすいのです。

<p align="center">
<img src="https://s3-ap-southeast-1.amazonaws.com/kipalog.com/pawct0wdxf_9HUTt.png" height="300" />
<br>
<em>Fig. 既存の同じコード行に2つの変更が発生した場合、マージコンフリクトイベントが発生します。<br>画像作：viblo.asia</em>
</p>

## 1. コンフリクトイベントをシミュレートする

このワークショップではマシンが1台しかないため、2台目としてGitHubのWebを採用します。GitHubのインターフェイスでリポジトリに変更を加えると、コンフリクトを発生させた2番目のユーザを表すことになります。ローカルにあるgitがコンフリクトを解決することになります。

1. GitHubのウェブインタフェースでファイルを変更し、ウェブ上でコミットします。

<p align="center">
<img src="../assets/basics_conflict-gitside.png" height="300" />
<br>
<em>Fig. 例えば、`onepiece.txt`から`boa`の行を削除するようにコミットしました。</em>
</p>

2. ローカルの git で、同じ行に別の変更を加えてください(例:`onepiece.txt`の`boa`行を`boa2`に変更します)。メッセージとともに、この変化をコミットしてください。

```plain
# ./onepiece.txt

luffy
nami
boa2
zoro
```

```bash
$ git log
# 
commit 5d4a94a51ca75c37df7fcc2f2a63287482a132e6 (HEAD -> main)
Author: nguyen.aislab <nguyen.aislab@gmail.com>
Date:   Wed Mar 1 17:11:51 2023 +0900

    update boa

...

$ git diff 5d4a94a~ 5d4a94a
diff --git a/onepiece.txt b/onepiece.txt
index fa97939..c4ff5b3 100644
--- a/onepiece.txt
+++ b/onepiece.txt
@@ -2,5 +2,5 @@
 
 luffy
 nami
-boa
+boa2
 zoro
```

3. このコミットをリモートレポにプッシュしてみてください。現在のHEADがリモートリポジトリの最新コミットより遅れていることが通知されます。

```bash
$ git push
To https://github.com/elchris97/ais-workshop23.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/elchris97/ais-workshop23.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

4. この問題を解決するには、リモートリポジトリから最新のHEAD情報を引き出してみてください。メッセージの意味は、「分岐したブランチがあるので、それらを調整する方法を指定する必要があります」。

```bash
$ git pull
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.

```

このケースは、Gitがリモートからの新しい変更と自分のローカルでの変更の違いを黙って解決できないという点で、これまでのシナリオとは異なります。

---

## 2. コンフリクトを解決する

その代わりに、GitはローカルとリモートのHEADの変更を組み合わせるための3つの戦略を提案しています。

```bash
$ git pull
...
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
...
```

1. 古典的なマージを行いたいので、まず `git config pull.rebase false` を実行してから、もう一度 `git pull` を試してみましょう。デフォルトのマージ戦略を3つの戦略から選択します。`merge`,`rebase`,`fast-forward` の3つのストラテジーから選択します。

*(他の2つの戦略は今日は説明しませんが、もし興味があればSlackから気軽に連絡してください。また、このトピックの別のクラスがあれば、ドキュメントを作成します)*

```bash
$ git config pull.rebase false
$ git pull
Auto-merging onepiece.txt
CONFLICT (content): Merge conflict in onepiece.txt
Automatic merge failed; fix conflicts and then commit the result. 
```

ここで、Git は `Merge` 戦略を使ってコンフリクトを解決します。コンフリクトはコンフリクトしたファイルの中で見つかります。どのファイルがコンフリクトしているのかを知るには、`git status`を使います。

2. 再度の引き込みに挑戦

```bash
$ git pull
Auto-merging onepiece.txt
CONFLICT (content): Merge conflict in onepiece.txt
Automatic merge failed; fix conflicts and then commit the result.
```

3. 今度は、Git がリモートリポから正常にプルされました。コンフリクトしている内容は、コンフリクトしているファイルから見つけることができました。コンフリクトしているファイルを表示するには

```bash
$ git status
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
 both modified:   onepiece.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

4. コンフリクトを解決し、リモートリポジトリにプッシュする前にマージコンフリクトを作成します。

```bash
$ cat onepiece.txt
# ./onepiece.txt

luffy
nami
<<<<<<< HEAD
boa2
=======
>>>>>>> c007cf6286c2759ff8f28996cccb85430711239e
zoro

```

<p align="center">
<img src="../assets/basics_conflict_rela.png" height="300" />
<br>
<em>Fig. コンフリクトしたファイルにおけるアノテーションの意味</em>
</p>

5. ローカルでの変更を維持したまま、新しい変更をコミットし、そのコミットを GitHub リポジトリにプッシュすることで、競合を解決してください。

```bash
$ cat onepiece.txt
# ./onepiece.txt

luffy
nami
boa2
zoro

$ git add onepiece.txt

$ git commit -m "対立を解決する"
[main cbca82d] 対立を解決する

$ git push
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 472 bytes | 472.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/elchris97/ais-workshop23.git
   c007cf6..cbca82d  main -> main
```

今回はプッシュが正常に実行されました。

---
[[MAIN]](../README.md) - [[BACK: Ch.2 Gitの基本コマンド]](./2-basics.md)