ユーザごとにN回目〜M回目に引いたガチャ結果を調べたくて見つけた関数。  
ランク（順位）を取得できる。


## 元データ

### gacha_results

|user_id|result|logged_at|
|:-:|:-:|:-:|
|1|N|2019-3-13 4:00:00|
|1|SR|2019-3-13 4:05:00|
|2|SR|2019-3-13 4:10:00|
|2|R|2019-3-13 4:15:00|
|1|N|2019-3-13 4:15:00|
|2|N|2019-3-13 4:20:00|
|3|R|2019-3-13 4:25:00|

## ユーザごとに2回目に引いた結果が見たい

### 期待する結果

|user_id|result|logged_at|
|:-:|:-:|:-:|
|1|SR|2019-3-13 4:05:00|
|2|R|2019-3-13 4:15:00|

### クエリ

まず、引いた順にランク付けする

```sql
select row_number() over (
  partition by user_id # ユーザごとに
  order by logged_at # 引いた順に
) as gacha_count,
user_id, gacha_result, logged_at
from gacha_results
;
```

↓

gacha_count|user_id|result|logged_at|
:-:|:-:|:-:|:-:|
1|1|N|2019-3-13 4:00:00|
2|1|SR|2019-3-13 4:05:00|
1|2|SR|2019-3-13 4:10:00|
2|2|R|2019-3-13 4:15:00|
3|1|N|2019-3-13 4:15:00|
3|2|N|2019-3-13 4:20:00|
1|3|R|2019-3-13 4:25:00|

上記結果を `gacha_count` で絞り込んで終わり

```sql
select user_id, gacha_result, logged_at
from (
  select row_number() over (
    partition by user_id
    order by logged_at
  ) as gacha_count,
  user_id, gacha_result, logged_at
  from gacha_results
)
where gacha_count = 2 # 2回目で絞り込む
;
```

↓

|user_id|result|logged_at|
|:-:|:-:|:-:|
|1|SR|2019-3-13 4:05:00|
|2|R|2019-3-13 4:15:00|

## まとめ
順序をつけるための関数を応用してN回目（個目）のレコードを取得できた。  
集計関数（というらしい）には他にも `rank()` , `dense_rank()` というのがあって、違いは以下の通り。

|value|rank()|dense_rank()|row_number()|
|:-:|:-:|:-:|:-:|
|10|1|1|1|
|20|2|2|2|
|20|2|2|3|
|25|4|3|4|
|30|5|4|5|

おわり
