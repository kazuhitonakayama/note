## [[heroku]]でのスキーマ変更のデプロイの後はrestartしたほうがいいかもしれない話
[Herokuの公式](https://devcenter.heroku.com/articles/rake)でも

```
After running a migration you'll want to restart your app with heroku restart to reload the schema and pickup any schema changes.

マイグレーションを実行した後は、heroku restartでアプリを再起動して、スキーマを再読み込みし、スキーマの変更をピックアップします。
```

とあって、これからマイグレーションファイルを更新した時は、
heroku restart
するのが良いかもしれない👀

別の記事で僕らと同じ問題に当たったことのある人の記事
- [Restart Heroku after a migration](https://coderwall.com/p/nfgmhg/restart-heroku-after-a-migration)  
- [heroku not updating database schema](https://stackoverflow.com/questions/4007425/heroku-not-updating-database-schema)