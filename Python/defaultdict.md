## 意義
辞書型のデータを扱う時の１番のリスクは「キーが存在しないこと」だと思います。キーが存在しないとエラーが発生し、エラーハンドリングをしっかりとしていないとそこで処理が強制終了されてしまいます。

今回紹介するdefaultdict()関数はキーが存在しないことによるエラーを防ぐのはもちろん、キーがない場合は新しい要素を追加してくれます。  
そのためキーの存在有無を気にせず実装をすることができ非常に便利です。

```python
>>> dict = collections.defaultdict(int)

>>> dict
# defaultdict(int)だとint()は0を返すので、keyがなかった場合のデフォルト値は0になる
defaultdict(<class 'int'>, {})

# なので0がhogeというkeyのvalueになる
>>> dict['hoge']
0

>>> dict
defaultdict(<class 'int'>, {'hoge': 0})

# ネストはできない
>>> dict['hoge']['hogehoge']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not subscriptable

>>> dict
defaultdict(<class 'int'>, {'hoge': 0})

>>> dict
defaultdict(<class 'int'>, {'hoge': 0})

>>> dict['hoga']
0

# 並列はできる
>>> dict
defaultdict(<class 'int'>, {'hoge': 0, 'hoga': 0})
```


## 参照
https://memoribuka-lab.com/?p=1680
https://qiita.com/xza/items/72a1b07fcf64d1f4bdb7

[[python]]