## fast forwardとは

```
featureブランチ:                             commit4 -> commit5
                                        /
masterブランチ：commit1 -> commit2 -> commit3 (HEAD)
```

みたいな時は、fast forward。
masterブランチの先っぽにfeatureブランチをくっつけたらいいだけ。この時マージコミットは作成されない！
master と hotfix が分岐しない連続的なヒストリのためこのようにすることができます

```
featureブランチ:               commit4 -> commit5
                            /
masterブランチ：commit1 -> commit2 -> commit3 (HEAD)
```

この時がまずい。
これは、ファストフォワードにならない。

## 参照
[gitのファストフォワードとは](https://yu8mada.com/2018/08/15/what-is-a-fast-forward-merge-in-git/)
[git pull と git pull –rebase の違いって？図を交えて説明します！](https://kray.jp/blog/git-pull-rebase/)

[[git]]