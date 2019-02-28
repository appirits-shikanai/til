# 複数のブランチを一度に削除する

`-d` コマンドに複数ブランチを指定できるのを知らなかった

```
‣ 🐌 git branch -d hoge fuga piyo
Deleted branch hoge (was xxxxxxxxx).
Deleted branch fuga (was xxxxxxxxx).
Deleted branch piyo (was xxxxxxxxx).
```

## 余談

Macのターミナルで `xargs` コマンドを使って一度に削除しようとしたら、余計な文字列が入ってしまい削除できなくて困っていた（なにか設定が悪いのかもだけど・色かな？）  
正規表現で置換とかはめんどくさい

```
‣ 🍤 git branch
  hoge1
  hoge2
  hoge3
* master
‣ 🍣 git branch | grep hoge | xargs git branch -d
error: branch 'hoge1?[m' not found.
error: branch 'hoge2?[m' not found.
error: branch 'hoge3?[m' not found. # ぐぬぬ
```

`xargs` を使うなら対象をコピペして `echo` して消す手もある

```
‣ 🐬 git branch | grep hoge
  hoge1
  hoge2
  hoge3
‣ 🐥 echo "  hoge1
>   hoge2
>   hoge3" | xargs git branch -d
Deleted branch hoge1 (was xxxxxxxxx).
Deleted branch hoge2 (was xxxxxxxxx).
Deleted branch hoge3 (was xxxxxxxxx).
```

なお `pbcopy` するとさっきの変な文字列が入る

```
‣ 🍡 git branch | grep hoge | pbcopy
‣ 🐳 echo `pbpaste`
hoge1 hoge2 hoge3 # いけそう！？
‣ 🌵 echo `pbpaste` | xargs git branch -d
error: branch 'hoge1?[m' not found.
error: branch 'hoge2?[m' not found.
error: branch 'hoge3?[m' not found. # いけない！
```

そもそも変な文字列入るのがおかしいからなんとかしたい  
おわり
