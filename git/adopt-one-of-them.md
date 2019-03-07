# コンフリクトしたときにどちらか一方のブランチを採用する

たまに使うのに毎回調べてしまうので書いておく

```
$ git branch --contains
* branchA
$ git merge branchB
--- なにかしらのコンフリクトが発生 ---

# branchA（今いるブランチ）を採用する
$ git checkout --ours file.txt
$ git add file.txt

# branchB（マージするブランチ）を採用する
$ git checkout --theirs file.txt
$ git add file.txt
```

おわり
