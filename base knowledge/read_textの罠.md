## read_textのソース

```python
def read_text(path, encoding=None):
    encoding = io.text_encoding(encoding)  # stacklevel=2
    with open(path, encoding) as f:
        return f.read()
```

そして、[openメソッド](https://docs.python.org/ja/3/library/functions.html#open)を使用すると、

>入力中の行は '\n', '\r', または '\r\n' で終わり、呼び出し元に返される前に '\n' に変換されます。

とある。  
なので、例え目的のTSCやCSVの改行コードが``\r\n``などであったとしても、そのファイルを``read_text()``で読み込んでしまうと、あたかも``\n``で改行されているように見えてしまう。

よって、この事態を避けるために``read_bytes()``で読み込み、``decode()``するアプローチが良さそう。

[TextIOWrapperとは](https://docs.python.org/ja/3/library/io.html#io.TextIOWrapper)



https://docs.python.org/3/library/functions.html#open