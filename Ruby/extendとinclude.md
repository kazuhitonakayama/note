## extend

[Ruby のモジュールの基礎](https://numb86-tech.hatenablog.com/entry/2019/03/23/225421)

```ruby
module SelfIntroduction
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction
end


User.who_am_i("Kazu")

# あるいは
module SelfIntroduction
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  self.who_am_i("Kazu")
end
```

```sh
> "I'm Kazu."
```

### ただし、privateメソッドは呼べない

```ruby
module SelfIntroduction
  private
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  User.who_am_i("Kazu")
end
```
けど、それでもこう書いたらいける

```ruby
module SelfIntroduction
  private
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  def self.hello
    who_am_i("Kazu")
  end

  User.hello
end
```

それか、``extend``じゃなくて``include``する

```ruby
module SelfIntroduction
  private
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  include SelfIntroduction

  def hello
    who_am_i("Kazu")
  end
end

User.new.hello
```

# 結論
``include``を使うと、モジュールで定義したメソッドを、自身のクラスのインスタンスメソッドとして使えるようになる。

```ruby
module SelfIntroduction
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  include SelfIntroduction

  def hello
    self.who_am_i("Kazu")
    # or 
    who_am_i("Kazu")
    # or 
    User.new.who_am_i("Kazu")
    # 既に、``User.new.hello`` でインスタンス化されているので、selfで実行できる
  end
end

User.new.hello
```

``extend``を使うと、、モジュールで定義したメソッドを、自身のクラスの特異メソッド（クラスメソッド）として使うことが出来る。

```ruby
module SelfIntroduction
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  def hello
    User.who_am_i("Kazu")
    # selfを使ってしまうと、エラー。なぜならインスタンス化 が既にされてしまっているので、クラスメソッドが機能しなくなる。#<User:xxxxdx>が既に生成されている
  end
end

User.new.hello
```

あるいは

```ruby
module SelfIntroduction
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  def self.hello
    self.who_am_i("Kazu")
    # あるいは
    User.who_am_i("Kazu")
    # あるいは
    who_am_i("Kazu")
    puts self
    # インスタンス 化がされてないから、selfはUserである。
  end
end

User.hello
```

もし、呼び出される側が``private``メソッドなら工夫する必要がある。

### include時
自身がインスタンスメソッドなら実行可能だが、
クラスメソッドなら難しい。
クラスメソッドなら自身でインスタンス化する必要があるが、インスタンス化時にprivateメソッドやから弾かれる。

```ruby
module SelfIntroduction
  private
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  include SelfIntroduction

  def hello
     who_am_i("Kazu")
     puts self
  end
end

User.new.hello
```

### extend時
自身がインスタンスメソッドなら実行が難しい、
クラスメソッドなら実行可能。
インスタンスメソッドが実行されるとインスタンスが生成されるが、extend時にはクラスメソッドになってしまうので、``User.new.who_am_i()``のようなことをしたいが、privateメソッドやから弾かれる。
クラスメソッドなら実行可能。

```ruby
module SelfIntroduction
  private
  def who_am_i(oneself)
    p "I'm #{oneself}."
  end
end

class User
  extend SelfIntroduction

  def self.hello
     who_am_i("Kazu")
  end
end

User.hello
```