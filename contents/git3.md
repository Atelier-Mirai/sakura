---
title: git switchとrestoreの役割と機能について
---

先日8/16にGitバージョン2.23.0がリリースされました。
今回の目玉機能と言えば、新しいコマンド `git switch` と `git restore` ですね！
本稿ではこちらの2つに絞ってどういう役割・位置づけの機能なのか英語ソースの引用も含めてご説明します。

##
 + ブランチの変更は `git switch`
 + ファイルの変更は `git restore`

**git switch**

```
git switch [<options>] [--no-guess] <branch>
git switch [<options>] --detach [<start-point>]
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

**restore**

```
git restore [<options>] [--source=<tree>] [--staged] [--worktree] <pathspec>…​
git restore (-p|--patch) [<options>] [--source=<tree>] [--staged] [--worktree] [<pathspec>…​]
```
### git switch
`git switch` はその名の通り、ブランチの変更を担当します。

```bash
# ブランチの切り替え
git switch <branch>

# ブランチを新規作成して切り替え
git switch -c <branch>
```

### git restore
`git restore` はファイルの変更に使用します。
ファイルを復旧させる時に使います。

```bash
# ファイルの変更を取り消す
git restore <filename>

# 特定ファイルを特定のコミットの状態にする
git restore --source <commit> <filename>
```

`git restore` コマンドは `git reset` のようにステージングエリアも変更出来るようになっています。

```bash
# ステージングエリアにあるファイルを復旧する（実ファイルへの変更はそのまま）
git restore --staged <filename>
```
