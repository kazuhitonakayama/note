## f文字列
Python3.6からf文字列（f-strings、フォーマット文字列、フォーマット済み文字列リテラル）という仕組みが導入され、冗長だった文字列メソッドformat()をより簡単に書けるようになった。

### かつてのPython
文字列メソッドformat()では、順番に引数を指定したり名前を決めてキーワード引数で指定したりして置換フィールド{}に値を挿入できる。

```python
a = 123

b = 'abc'

print('{} and {}'.format(a, b))
# 123 and abc

print('{first} and {second}'.format(first=a, second=b))
# 123 and abc
```

### 3.6からのPython
f文字列（f-strings）は[[文字列リテラル]]の前にfまたはFを置く（f'xxx', F'xxx'）。文字列中の置換フィールドに変数をそのまま指定できる。

```python
print(f'{a} and {b}')
# 123 and abc

print(F'{a} and {b}')
# 123 and abc
```

## 参考
https://note.nkmk.me/python-f-strings/