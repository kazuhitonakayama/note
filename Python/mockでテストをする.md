## S3をモックする
boto3をモックするから
motoがある？

まあいいのよ。
何しか、motoであのテーションを関数などの前につけることでその関数内でのS3操作などはモック化できたりする。

```python
@mock_s3
def test_upload_file_to_s3:
```

## ローカルをモックする
正確にはローカルの一時ディレクトリを参照している。
``tmp_path``や``tmpdir``などがある。

[pythonヘビーユーザーへの道](https://www.m3tech.blog/entry/pytest-summary)

[pytest 公式ドキュメント](https://docs.pytest.org/en/6.2.x/tmpdir.html)

[[python]]