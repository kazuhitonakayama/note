## luigi
[m3のブログ](https://www.m3tech.blog/entry/2018/10/17/105115)が良さそう。

基本的に構文は3つで
- requires()
  - 依存している他のTaskを返す。このタスクのrunが呼ばれる前に、この関数が返すTaskのrunが呼ばれます。
- output()
  - Taskの出力先。luigi.TargetやそのList、Dictになります。
- run()
  - Taskの実行ロジックを定義する。inputとして、requiresのタスクのoutputが渡される。何かしらの処理を行い、output先に出力します。

[[python]]