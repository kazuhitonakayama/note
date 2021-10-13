## copy

```python
a = [[1, 2], [3, 4]]
b = copy.copy(a)
a[1].append(5)

print ("a = %s" % a)
print ("b = %s" % b)

print ("id(a) = %i" % id(a))
print ("id(b) = %i" % id(b))

print ("id(a[0]) = %i" % id(a[0])) # 追加行
print ("id(b[0]) = %i" % id(b[0])) # 追加行

print ("id(a[1]) = %i" % id(a[1])) # 追加行
print ("id(b[1]) = %i" % id(b[1])) # 追加行

# 実行結果
a = [[1, 2], [3, 4, 5]]
b = [[1, 2], [3, 4, 5]]

id(a) = 140092866607048
id(b) = 140092739841480

id(a[0]) = 140092739841352
id(b[0]) = 140092739841352

id(a[1]) = 140092841262536
id(b[1]) = 140092841262536
```

この通り、変数の中身、２次元配列の内部配列については、同じIDを指していることがわかります。
これを防ぐには、deepcopyを使う必要があります。
## deepcopy

```python
a = [[1, 2], [3, 4]]
b = copy.deepcopy(a) #変更行
a[1].append(5)

print ("a = %s" % a)
print ("b = %s" % b)

print ("id(a) = %i" % id(a))
print ("id(b) = %i" % id(b))

print ("id(a[0]) = %i" % id(a[0]))
print ("id(b[0]) = %i" % id(b[0]))

print ("id(a[1]) = %i" % id(a[1]))
print ("id(b[1]) = %i" % id(b[1]))

# 実行結果
a = [[1, 2], [3, 4, 5]]
b = [[1, 2], [3, 4]]

id(a) = 140092840880264
id(b) = 140092728434760

id(a[0]) = 140092840879752
id(b[0]) = 140092728721352

id(a[1]) = 140092728396104
id(b[1]) = 140092867894216
```

このとおり。配列の中まで全てオブジェクトIDが異なっています。

## reference
https://qiita.com/Kaz_K/items/a3d619b9e670e689b6db

Python公式Document  
https://docs.python.org/ja/3/library/copy.html
[[python]]